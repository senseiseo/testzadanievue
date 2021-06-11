<template>
  <div>
   <h1> {{timeValue}} </h1>

     <button type="button" id="add-request" v-on:click="openAddPopup">Добавить заявку</button>
     <br>
     <br>
     <table class="filters__table" style="width: 600px;">

              <thead style="background: #fc18">
               <tr>
                <td> Организация 
                  <button type="button" class="filters__button">BUTTON</button>
                </td>
                <td> Статус 
                  <button type="button" class="filters__button">BUTTON</button>
                </td>
                <td> Приоритет 
                  <button type="button" class="filters__button">BUTTON</button>
                </td>
                <td> Тип 
                  <button type="button" class="filters__button">BUTTON</button>
                </td> 
                <td> Заявитель 
                  <button type="button" class="filters__button">BUTTON</button>
                </td>
                <td> Наименование 
                  <button type="button" class="filters__button">BUTTON</button>
                </td> 
                <td> Исполнитель 
                  <button type="button" class="filters__button">BUTTON</button>
                </td>
                <td> Время 
                  <button type="button" class="filters__button">BUTTON</button>
                </td>
               </tr> 
              </thead>

              <tbody  style="background: #ccc">
               <tr v-for="(item, i) in items" v-bind:key="item.id">
                <td> {{item.organization}} </td>
                <td> {{item.status}} </td>
                <td> {{item.priority}} </td>
                <td> {{item.type}} </td>
                <td> {{item.applicant}} </td>
                <td> {{item.name}} </td> 
                <td> {{item.performer}} </td>
                <td> {{item.time}} </td>
                <button :data-id="item.id" type="button" class="edit" v-bind:id="item.id" @click="openEditRequestForm">Редактировать заявку</button>
                <button type="button" class="delete" @click="deleteRequest(i)">Удалить заявку</button>
               </tr> 
              </tbody>
             </table>

             <div class="edit-request hidden" id="edit-request-popup">
              <form class="edit-request__form" @submit.prevent="editRequest" id="popup-form">
                <p class="edit-request__item">
                  <label for="organization">Организация</label>
                  <input type="text" id="organization" name="organization" :value="currentRow.organization" required>
                </p>
                <p class="edit-request__item">
                  <label for="status">Статус</label>
                  <input type="text" id="status" name="status" :value="currentRow.status" required>
                </p>
                <p class="edit-request__item">
                  <label for="priority">Приоритет</label>
                  <input type="text" id="priority" name="priority" :value="currentRow.priority" required>
                </p>
                <p class="edit-request__item">
                  <label for="type">Тип</label>
                  <input type="text" id="type" name="type" :value="currentRow.type" required>
                </p>
                <p class="edit-request__item">
                  <label for="applicant">Заявитель</label>
                  <input type="text" id="applicant" name="applicant" :value="currentRow.applicant" required>
                </p>
                <p class="edit-request__item">
                  <label for="name">Наименование</label>
                  <input type="text" id="name" name="name" :value="currentRow.name"  required>
                </p>
                <p class="edit-request__item">
                  <label for="performer">Исполнитель</label>
                  <input type="text" id="performer" name="performer" :value="currentRow.performer" required>
                </p>
                <p class="edit-request__item">
                  <label for="time">Время</label>
                  <input type="time" id="time" name="time" :value="currentRow.time" required>
                </p>
                  <input type="hidden" name="id" :value="currentRow.id">
                <button id="request-edit-btn" type="submit">Редактировать запись</button>
                <button type="button" @click="closePopup">Закрыть</button>
              </form>

             </div>
             <div class="add-request hidden" id="add-request-popup">
             <form class="add-request__form" @submit.prevent="addRequest" id="add-popup-form">
               <p class="add-request__item">
                 <label for="organization">Организация</label>
                 <input type="text" id="organization" name="organization" :value="organization" required>
               </p>
               <p class="add-request__item">
                 <label for="status">Статус</label>
                 <input type="text" id="status" name="status" :value="status" required>
               </p>
               <p class="add-request__item">
                 <label for="priority">Приоритет</label>
                 <input type="text" id="priority" name="priority" :value="priority" required>
               </p>
               <p class="add-request__item">
                 <label for="type">Тип</label>
                 <input type="text" id="type" name="type" :value="type" required>
               </p>
               <p class="add-request__item">
                 <label for="applicant">Заявитель</label>
                 <input type="text" id="applicant" name="applicant" :value="applicant" required>
               </p>
               <p class="add-request__item">
                 <label for="name">Наименование</label>
                 <input type="text" id="name" name="name" :value="name" required>
               </p>
               <p class="add-request__item">
                 <label for="performer">Исполнитель</label>
                 <input type="text" id="performer" name="performer" :value="performer" required>
               </p>
               <p class="add-request__item">
                 <label for="time">Время</label>
                 <input type="time" id="time" name="time" :value="time" required>
               </p>
                 <input type="hidden" name="id" :value="id">
               <button id="add-request-btn" type="submit">Добавить запись</button>
               <button type="button" @click="closePopup">Закрыть</button>
             </form>
            </div>
  </div>
  
</template>

<script>
export default {
  name: 'TestTable',
 
  data() {
    return {
      // создаем массив данных
      
     items: [           
       {
         id: 1,
         organization: 'Компания 1',
         status: 'Принята',
         priority: 'Средний',
         type: 'Сообщение об ошибке',
         applicant: 'Иванов',
         name: 'Тестовая заявка',
         performer: 'Петров',
         time: '12:22'
       },
       {
         id: 2,
         organization: 'Компания 2',
         status: 'Отказано',
         priority: 'Низкий',
         type: 'Сообщение об улучшении',
         applicant: 'Махмутов',
         name: 'Тестовая заявка',
         performer: 'Шайдуллин',
         time: '12:48'
       },
       {
         id: 3,
         organization: 'Компания 3',
         status: 'Принята',
         priority: 'Высокий',
         type: 'Заявка на установку',
         applicant: 'Максимов',
         name: 'Тестовая заявка',
         performer: 'Ахметов',
         time: '12:33'
       }
     ],
     //создаем текущий массив для создание новой заявки
     currentRow: {
       id: '',
       organization: '',
       status: '',
       priority: '',
       type: '',
       applicant: '',
       name: '',
       performer: '',
       time: ''
     }
    }
  },

  methods: {
    // окно для редактирования данных 
    openAddPopup() {
      let popup = document.getElementById('add-request-popup');
      popup.classList.remove('hidden');
    },
    // Добавление заявки
    addRequest() {
      let form = document.getElementById('add-popup-form'); 
      let lastId = this.items[this.items.length - 1].id + 1;
      console.log(lastId)
      let id = lastId;
      let organization = form.elements.organization.value;
      let status = form.elements.status.value;
      let priority = form.elements.priority.value;
      let type = form.elements.type.value;
      let applicant = form.elements.applicant.value;
      let name = form.elements.name.value;
      let performer = form.elements.performer.value;
      let time = form.elements.time.value;
      let obj = {
        id, organization, status, priority, type, applicant, name, performer,time
      };
      this.items.push(obj);
    },
    deleteRequest(i)
    // удаление заявки 
    {
       console.log('deleteRequest', i)
      this.items.splice(i, 1)
     
    //  event.target.parentNode.parentNode.removeChild(event.target.parentNode);
    },
    editRequest()
    // редактирование заявки 
    {
      let form = document.getElementById('popup-form');
      let id = form.elements.id.value;
      let organization = form.elements.organization.value;
      let status = form.elements.status.value;
      let priority = form.elements.priority.value;
      let type = form.elements.type.value;
      let applicant = form.elements.applicant.value;
      let name = form.elements.name.value;
      let performer = form.elements.performer.value;
      let time = form.elements.time.value;
      let obj = {
        id, organization, status, priority, type, applicant, name, performer,time
      };
      for(let i in this.items) {
        if(obj.id == this.items[i].id) {
          this.items[i] = obj;
          let popup = document.getElementById('edit-request-popup');
          popup.classList.add('hidden');
        } 
      }
      
      // popup.classList.add('hidden');
    },
    // открыытие формы для редактирование заявки
    openEditRequestForm(event) {
      let popup = document.getElementById('edit-request-popup');
      if(popup.classList.contains('hidden')) {
        popup.classList.remove('hidden');
      }
      let obj = this.items[event.target.id - 1];
      this.currentRow.id = obj.id;
      this.currentRow.organization = obj.organization;
      this.currentRow.status = obj.status;
      this.currentRow.priority = obj.priority;
      this.currentRow.type = obj.type;
      this.currentRow.applicant = obj.applicant;
      this.currentRow.name = obj.name;
      this.currentRow.performer = obj.performer;
      this.currentRow.time = obj.time;
    },
    //закрытие окна редактирования окна
    closePopup(event) {
      let popup = document.getElementById(event.target.parentNode.parentNode.id);
      if(!popup.classList.contains('hidden')) {
        popup.classList.add('hidden');
      }
    }
  }
}

</script>

<style>
  .filters__table {
    margin: 0 auto;
  }
  .edit {
    background-color: green;
  }
  .delete {
    background-color: red;
  }
  .edit-request,
  .add-request {
    z-index: 3;
    position: fixed;
    top: 20%;
    left: 38%;
    width: 350px;
    min-height: 60px;
    border: 1px solid black;
    background-color: white;
    opacity: 0.95;
  }

  .hidden {
    display: none;
  }
  .edit-request__form,
  .add-request__form {
    width: 200px;
    margin: 0 auto;
  }
  
</style>