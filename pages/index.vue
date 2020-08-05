<template>
  <div class="home">
    <h2>Oskars Barbers Administrative Page</h2>
    <form class="login" v-if="!loggedIn" onSubmit="return false">
      <ul>
        <li>
          <label for="">username</label>
          <input type="text" v-model="login.username">
        </li>
        <li>
          <label for="">password</label>
          <input type="password" v-model="login.password" >
          <!-- <input type="text" v-model="login.password" @keyup.prevent="checkEnter($event)"> -->
        </li>
        <li class="actions">
          <label for=""></label>
          <button @click.prevent="clear">Clear</button>
          <button @click.prevent="validate">Submit</button>
        </li>
      </ul>
      <p>{{formError}}</p>

      <!-- <div class="shown-1">
        <span class="val-1">test</span>  <button @click="show(1)">edit</button>
      </div>
      <div class="hidden hidden-1">
        <input type="text" value="test" class="update-1"  > <button @click="hide(1)">submit</button>
      </div>

      <div class="shown-2">
        <span class="val-2">test2</span>  <button @click="show(2)">edit</button>
      </div>
      <div class="hidden hidden-2">
        <input type="text" value="test2" class="update-2" > <button @click="hide(2)">submit</button>
      </div> -->
      
    </form>
    <div v-else class="loggedIn">
      <div>
        <button class="logout" @click.prevent="logout">Log out</button>

        <p v-for="(p,index) in prices" :key="index">
          <ul :class="'shown-' + p.id">
            <li>
              <label for="">item</label>
              <span :class="'valItem-' + p.id">{{p.item}}</span>
            </li>
            <li>
              <label for="">price</label>
              <span :class="'valPrice-' + p.id">{{p.price}}</span> <iconEdit @click="show(p.id)" />
            </li>
          </ul>

          <ul class="hidden" :class="'hidden-' + p.id">
            <li>
              <label for="">item</label>
              <input type="text" :value="p.item" class="item" :class="'updateItem-' + p.id">
            </li>
            <li>
              <label for="">price</label>
              <input type="text" :value="p.price"  class="price" :class="'updatePrice-' + p.id"> 
              <iconSend @click="update(p.id)"/>
            </li>
          </ul>
        </p>
          <!-- <li>
            <label for="">item</label>
            <input type="text" :value="p.item" @blur="edit_item(p.id,$event)" class="item">
          </li>
          <li>
            <label for="">price</label>
            <input type="text" :value="p.price" @blur="edit_price(p.id,$event)" class="price">
          </li> -->
      </div>
      <section>
        <ul>
        Add new item
          <li>
            <label for="">item</label>
            <input type="text" v-model="newItem" class="item">
          </li>
          <li>
            <label for="">price</label>
            <input type="text" v-model="newItemPrice" class="price">
          </li>
          <li>
            <label for=""></label>
            <button @click.prevent="add">Add</button>
          </li>
        </ul>
      </section>
    </div>
  </div>
</template>

<script>
import iconEdit from 'vue-material-design-icons/Pencil.vue'
import iconSend from 'vue-material-design-icons/Send.vue'
export default {
  components: { iconEdit, iconSend},
  data() {
    return {
      login :{
        loginToOskars: '',
        username: 'adam',
        password: '8787os',
      },
      formError: '',
      loggedIn : false,
      loginStatus: '',
      prices: '',
      url: 'https://oskarsbarbers4men.co.uk/indexAPI.php',
      newItem: '', newItemPrice: '',
    }
  },
  methods: {
    show(n) {
      document.querySelector('.hidden-' + n).style.display = 'inline-block'
      document.querySelector('.shown-' + n).style.display = 'none'
    },
    update(id) {
      let item = document.querySelector('input.updateItem-' + id).value 
      let price = document.querySelector('input.updatePrice-' + id).value 
      let info = { editItemPrice : true, item, price, id }
      console.log(info)
      this.$axios.post(this.url, info)
      .then( res => {
        console.log(res.data)
        // if(res.data == 'success : edited ItemPrice') {
        //   alert('updated ' + id)
        // }
      })
      document.querySelector('.hidden-' + id).style.display = 'none'
      document.querySelector('.shown-' + id).style.display = 'block'
      document.querySelector('.valItem-' + id).innerHTML = item
      document.querySelector('.valPrice-' + id).innerHTML = price
    },
    clear() {
      this.login.username = '', this.login.password = '', this.formError  = ''
    },
    clearNew() {
      this.newItem = '', this.newItemPrice = ''
    },
    dont() {
      console.log('')
    },
    checkEnter(e) {
      if (e.keyCode === 13) {
        // e.key = null
        e.preventDefault();
        // this.validate()
        return
      }
    },
    validate() {
      this.formError = ''
      if(this.login.username == '' || this.login.password == '') {
        this.formError = 'pls fill out both fields'
        console.log('errors')
      } else {
        this.submit()
      }
    },
    submit() {
      this.$axios.post(this.url, this.login)
      .then(res => {
        if(res.data == 'success') {
          this.loggedIn = true
          localStorage.setItem('loggedIn', true)
          this.successful_login()
          console.log(res.data)
        } else {
          this.formError = 'login error'
        }
      })
    },
    successful_login() {
      this.$axios.get(this.url)
      .then(res => {
        this.prices = res.data
        })
    },
    logout() {
      this.loggedIn = false
      localStorage.removeItem('loggedIn')
      this.$axios.get(this.url + '?logout')
      // setTimeout(() => {
      //   window.location.reload()      
      // }, 500);
    },
    edit_item(i,e) {
      this.checkLoginStatus()
      setTimeout(() => {
        if(this.loginStatus == 'ok') {
          let update = { editItem: true, id: i, item: e.target.value}
          console.log(update)
          this.$axios.post(this.url, update)
          .then(res => {
            console.log(res.data)
          })  
        }        
      }, 1000);
    },
    edit_price(i,e) {
      this.checkLoginStatus()
      setTimeout(() => {
        if(this.loginStatus == 'ok') {
          let update = { editPrice: true, id: i, price: e.target.value}
          console.log(update)
          this.$axios.post(this.url, update)
          .then(res => {
            console.log(res.data)
          })  
        }        
      }, 1000);
    },
    checkLoginStatus() {
      this.$axios.get(this.url + '?check_status')
      .then(res => {
        console.log(res.data)
        if(res.data == 'loggedin') {
          console.log("you're still logged in")
          this.loginStatus = 'ok'
          } else {
          this.logout()
          window.location.reload()
        }
      })
    },
    add() {
      let newItem =  {addItem: true, item: this.newItem, price: this.newItemPrice}
      console.log(newItem)
      this.$axios.post(this.url, newItem)
      .then(res => {
        console.log(res.data)
      })
      setTimeout(() => {
        this.successful_login()
        this.clearNew()
      }, 1000);
    }
  },
  mounted() {
    this.$axios.get(this.url + '?check_status')
    .then(res => {
      if(res.data == 'loggedin') {
        localStorage.setItem('loggedIn', true)
        this.loggedIn = true
        this.successful_login()
      } else {
        this.logout()        
      }
    })
  }
}
</script>

