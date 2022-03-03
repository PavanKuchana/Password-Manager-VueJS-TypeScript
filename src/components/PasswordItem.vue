<template>
<div class="password-details-container">
   <div class="initial-container">
        <p class="initial-text">{{ initialLetter[0].toUpperCase() }}</p>
    </div>
    <div class="user-entered-details">
        <p class="details">{{ details.website }}</p>
        <p class="details">{{ details.username }}</p>
        <div v-if="showPasswords">
        <p class="password">{{ details.password }}</p>
        </div>
        <div v-else>
        <img src="https://assets.ccbp.in/frontend/react-js/password-manager-stars-img.png" alt="stars" class="stars-image" />
        </div>
        
    </div>

    <button class="delete-button" type="button" testid="delete" @click="$emit('deleteItem',details.id)">
        <img src="https://assets.ccbp.in/frontend/react-js/password-manager-delete-img.png" alt="delete" class="delete-icon" />
    </button>
</div>
</template>



<script lang="ts">
import { defineComponent, PropType, reactive } from 'vue'

type data ={
    id : number,
    website :string,
    password:string ,
    username:string
}

export default defineComponent({
    name:'PasswordItem',
    props:{
          passwordDetails:{
            type:Object as PropType<data>,
            required:true
          },
        showPasswords : Boolean as any
    },
    setup(props){
        
        const details = reactive({...props.passwordDetails});

        const initialLetter = details.username.split('').slice(0, 1)
        return {initialLetter,details}
    }
})
</script>


<style scoped>
.password-details-container {
  display: flex;
  align-items: center;
  justify-content: space-between;
  border: 1px solid #f8fafc;
  padding: 10px;
  width: 100%;
  border-radius: 10px;
  margin: 0px 10px 10px 0px;
}

@media screen and (min-width: 768px) {
  .password-details-container {
    width: 290px;
    height: 150px;
    margin-right:20px;
  }
}

.initial-container {
  display: flex;
  justify-content: center;
  align-items: center;
  width: 50px;
  height: 50px;
  background-color: #0ea5e9;
  border-radius: 25px;
  color: #f8fafc;
  font-family: 'Roboto';
  font-size: 24px;
  margin-right:20px;
}

.initial-text {
  color: #ffffff;
}

.user-entered-details {
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: flex-start;
  min-height: 80px;
  margin-right:20px;
}

.details {
  margin: 1px;
  font-family: 'Roboto';
  color: #f8fafc;
}

.password{
  color:#000000;
  font-weight: 500;
}

.delete-button {
  display: flex;
  justify-content: center;
  align-items: center;
  width: 30px;
  background-color: transparent;
  border: none;
  outline: none;
  cursor: pointer;
  margin-left: 30px;
}

.delete-icon {
  width: 30px;
}

.stars-image {
  width: 120px;
  margin-top: 5px;
}
</style>