<template>
      <div class="bg-container">
        <div class="password-manager-container">
          <img
            src="https://assets.ccbp.in/frontend/react-js/password-manager-logo-img.png"
            alt="app logo"
            class="app-logo"
          />
          <div class="all-inputs-container">
            <img
              src="https://assets.ccbp.in/frontend/react-js/password-manager-sm-img.png"
              alt="password manager"
              class="main-image"
            />
            <form class="form-container" @submit="onAddPassword">
              <h1 class="heading">Add New Password</h1>
              <div class="input-container">
                <div class="input-logo-icon">
                  <img
                    src="https://assets.ccbp.in/frontend/react-js/password-manager-website-img.png"
                    alt="website"
                    class="logo"
                  />
                </div>
                <div class="input-element">
                  <input
                    type="url"
                    placeholder="Enter Website"
                    class="input"
                    @change="onChangeWebsite($event)"
                    v-model="website"
                  />
                </div>
              </div>
              <div class="input-container">
                <div class="input-logo-icon">
                  <img
                    src="https://assets.ccbp.in/frontend/react-js/password-manager-username-img.png"
                    alt="username"
                    class="logo"
                  />
                </div>
                <div class="input-element">
                  <input
                    type="text"
                    placeholder="Enter Username"
                    class="input"
                    @change="onChangeUsername($event)"
                    v-model="username"
                  />
                </div>
              </div>
              <div class="input-container">
                <div class="input-logo-icon">
                  <img
                    src="https://assets.ccbp.in/frontend/react-js/password-manager-password-img.png"
                    alt="password"
                    class="logo"
                  />
                </div>
                <div class="input-element">
                  <input
                    type="password"
                    placeholder="Enter Password"
                    class="input"
                     @change="onChangePassword($event)"
                    v-model="password"
                  />
                </div>
              </div>
              <div class="button-container">
                <button type="submit" class="add-button">
                  Add
                </button>
              </div>
            </form>
          </div>
          <div class="output-container">
            <div class="top-section">
              <div class="counter-section">
                <h1 class="your-password">Your Passwords</h1>
                <div class="counter">
                  <p class="count">{{ passwordsList.length }}</p>
                </div>
              </div>
              <div class="input-container search-element">
                <div class="input-logo-icon">
                  <img
                    src="https://assets.ccbp.in/frontend/react-js/password-manager-search-img.png"
                    alt="search"
                    class="logo"
                  />
                </div>
                <div class="input-element">
                  <input
                    type="search"
                    placeholder="Search"
                    class="input"
                    v-model="search"
                    @change="onChangeSearchInput($event)"
                  />
                </div>
              </div>
            </div>
            <hr class="line" />
            <div class="toggle-view">
              <label></label>
              <input
                id="checkBox"
                type="checkbox"
                class="check"
                @click="onChangePasswordView"
                v-model="showPasswords"
              />
              <label htmlFor="checkBox" class="show-password">
                Show Passwords
              </label>
            </div>
            <div class="output-passwords-list" v-if="newResults.length">
                <div :key ="each.id" v-for="each in newResults">
                    <PasswordItem 
                         :passwordDetails="each"
                         :showPasswords="showPasswords"
                         @deleteItem="deleteItem"
                    />
                </div>
            </div>
            <div v-else>
                <div class="no-passwords-container">
                <img
                  src="https://assets.ccbp.in/frontend/react-js/no-passwords-img.png"
                  alt="no passwords"
                  class="no-passwords-image"
                />
                <p class="no-passwords">No Passwords</p>
              </div>
            </div>
          </div>
        </div>
      </div>
    
</template>

<script lang="ts">
import PasswordItem from './PasswordItem.vue'
import { computed, ref,defineComponent } from 'vue';

type data={
    id : number,
    website :string,
    password:string,
    username:string
}

export default defineComponent({
    name:"PasswordManager",
    components:{
        PasswordItem
    },
    setup(){

        const passwordsList = ref<data[]>([])
        
        const id = ref()

        const website = ref()

        const username = ref('')

        const password = ref('')

        const search = ref('')
        
        const showPasswords = ref(false)

        const onChangeWebsite = (event:any) => {
            website.value = event.target.value
            
        }

        const onChangeUsername = (event:any) => {
            username.value = event.target.value
            
        }

        const onChangePassword = (event:any) => {
            password.value = event.target.value
            
        }

        const onChangeSearchInput = (event:any) => {
            search.value = event.target.value
            search.value =''
        }


        const  onChangePasswordView = () => {
            showPasswords.value = !showPasswords.value
       }

        
         const onAddPassword = (event:any) => {
          event.preventDefault()
          if (!website.value || !username.value || !password.value ) {
            alert("Please enter all the fields")
          }
          else{
              const newItem:data ={
              id : Math.floor(Math.random()*100000),
              website : website.value,
              username : username.value,
              password :password.value
            }
            passwordsList.value = [...passwordsList.value,newItem]
              website.value = '',
              username.value = '',
              password.value = ''
            
            }
       
          }
        const newResults = computed(()=>{
            return passwordsList.value.filter(eachResult =>
        eachResult.username.toLowerCase().includes(search.value.toLowerCase()),
          )
          })

        const deleteItem = (id:number)=>{
          if(confirm('Are you sure ??')){
            passwordsList.value= passwordsList.value.filter(eachItem => eachItem.id !== id)
          }
        }
  

      
  return {deleteItem,newResults,onAddPassword,
          onChangePasswordView,showPasswords,passwordsList,
          website,username,password,search,onChangeWebsite,
          onChangeUsername,onChangePassword,onChangeSearchInput
          }

  }
})
</script>

<style scoped>
.bg-container {
  background-image: linear-gradient(#9ba9eb, #c3caea);
  min-height: 100vh;
  background-size: cover;
  display: flex;
  justify-content: center;
  padding-top: 40px;
}

.password-manager-container {
  width: 90%;
  display: flex;
  flex-direction: column;
  align-items: center;
}

.app-logo {
  align-self: flex-start;
  width: 180px;
}

@media screen and (min-width: 768px) {
  .password-manager-container {
    width: 80%;
  }

  .app-logo {
    width: 200px;
  }
}

.all-inputs-container {
  background-color: #5763a5;
  width: 100%;
  margin-top: 20px;
  border-radius: 10px;
  display: flex;
  flex-direction: column;
  align-items: center;
  padding: 20px;
  margin-bottom: 20px;
}

.main-image {
  width: 90%;
}

@media screen and (min-width: 576px) {
  .main-image {
    width: 300px;
  }
}

@media screen and (min-width: 768px) {
  .all-inputs-container {
    flex-direction: row;
    justify-content: space-between;
    padding: 50px 30px 50px 30px;
  }

  .main-image {
    order: 1;
    width: 300px;
  }
}

.form-container {
  background-color: #454f84;
  width: 100%;
  padding: 20px 10px 20px 10px;
  height: 100%;
  display: flex;
  flex-direction: column;
  align-items: center;
  border-radius: 20px;
  margin-top: 15px;
  margin-right: 0px;
}

@media screen and (min-width: 768px) {
  .form-container {
    min-width: 220px;
    max-width: 350px;
    margin-top: 0px;
    margin-right: 20px;
  }
}

.heading {
  font-family: 'Roboto';
  color: #f8fafc;
  font-weight: 400;
  width: 90%;
}

.input-container {
  display: flex;
  align-items: center;
  width: 90%;
  height: 40px;
  margin-bottom: 10px;
}

.logo {
  width: 30px;
  margin: 0px;
}

.input-logo-icon {
  background-color: #f8fafc;
  height: 100%;
  display: flex;
  justify-content: center;
  align-items: center;
  width: 20%;
  border: 1px solid #94a3b8;
  border-top-left-radius: 5px;
  border-bottom-left-radius: 5px;
}

.input-element {
  background-color: #f8fafc;
  height: 100%;
  display: flex;
  justify-content: center;
  align-items: center;
  width: 80%;
  border: 1px solid #94a3b8;
  border-top-right-radius: 5px;
  border-bottom-right-radius: 5px;
}

.input {
  border: none;
  outline: none;
  width: 90%;
}

.button-container {
  width: 90%;
  display: flex;
  justify-content: flex-end;
}

.add-button {
  background-color: #0b69ff;
  border: none;
  outline: none;
  cursor: pointer;
  color: #ffffff;
  padding: 10px;
  width: 80px;
  font-family: 'Roboto';
  border-radius: 5px;
}

.output-container {
  background-color: #5763a5;
  width: 100%;
  display: flex;
  flex-direction: column;
  border-radius: 10px;
  padding: 20px;
  min-height: 300px;
  margin-bottom: 50px;
}

.top-section {
  display: flex;
  justify-content: space-between;
  align-items: center;
  justify-content: space-between;
  width: 100%;
  margin-bottom: 10px;
}

.counter-section {
  display: flex;
  align-items: center;
  width: 50%;
}

.search-element {
  width: 50%;
  display: flex;
  align-items: center;
  justify-content: flex-end;
}

@media screen and (min-width: 768px) {
  .output-container {
    padding: 30px 30px 50px 30px;
  }

  .counter-section {
    width: 300px;
  }

  .search-element {
    width: 30%;
  }
}

.your-password {
  margin-right: 5px;
  font-family: 'Roboto';
  color: #f8fafc;
  font-size: 14px;
  font-weight: 500;
}

.counter {
  border: 1px solid #f8fafc;
  background-color: transparent;
  width: 40px;
  height: 20px;
  color: #f8fafc;
  font-weight: 500;
  display: flex;
  justify-content: center;
  align-items: center;
  border-radius: 10px;
}

@media screen and (min-width: 768px) {
  .your-password {
    font-size: 20px;
  }
}

.count {
  margin: 0px;
}

.line {
  border: 1px solid #f8fafc;
  width: 100%;
}

.output-passwords-list {
  display: flex;
  flex-direction: column;
  align-items: center;
  list-style-type: none;
  width: 100%;
  margin-top:20px;
  padding-left: 0px;
}

@media screen and (min-width: 768px) {
  .output-passwords-list {
    flex-direction: row;
    align-items: center;
    flex-wrap: wrap;
  }
}

.toggle-view {
  display: flex;
  align-items: center;
  width: 100%;
  justify-content: flex-end;
}

.check {
  width: 20px;
  height: 20px;
  cursor: pointer;
}

.show-password {
  font-family: 'Roboto';
  color: #f8fafc;
  font-weight: 500;
}

.no-passwords-container {
  width: 100%;
  display: flex;
  flex-direction: column;
  align-items: center;
}

.no-passwords-image {
  width: 300px;
}

.no-passwords {
  color: #f8fafc;
  font-family: 'Roboto';
  font-weight: 500;
  font-size: 20px;
}
</style>