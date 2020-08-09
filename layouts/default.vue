<template>
  <div class="wrapper">
    <div class="content">
      <Nuxt />
    </div>
    <appFooter />
  </div>
</template>
<script>
import appFooter from '~/components/footer'
export default {
  components: { appFooter }
}
</script>

<style lang="scss">
html {
  font-family:
    'Source Sans Pro',
    -apple-system,
    BlinkMacSystemFont,
    'Segoe UI',
    Roboto,
    'Helvetica Neue',
    Arial,
    sans-serif;
  font-size: 16px;
  word-spacing: 1px;
  -ms-text-size-adjust: 100%;
  -webkit-text-size-adjust: 100%;
  -moz-osx-font-smoothing: grayscale;
  -webkit-font-smoothing: antialiased;
  box-sizing: border-box;
}

*,
*::before,
*::after {
  box-sizing: border-box;
  margin: 0;
}

.button--green {
  display: inline-block;
  border-radius: 4px;
  border: 1px solid #3b8070;
  color: #3b8070;
  text-decoration: none;
  padding: 10px 30px;
}

.button--green:hover {
  color: #fff;
  background-color: #3b8070;
}

.button--grey {
  display: inline-block;
  border-radius: 4px;
  border: 1px solid #35495e;
  color: #35495e;
  text-decoration: none;
  padding: 10px 30px;
  margin-left: 15px;
}

.button--grey:hover {
  color: #fff;
  background-color: #35495e;
}

html {
  overflow: auto;
}
.wrapper {
  display: flex;
  flex-direction: column;
  height: 100vh;
}
.content {
  flex: 1 0 auto;
  background-color: #1172f04f;
}
footer {
  flex-shrink: 0;
  display: flex;
  justify-content: center;
  line-height: 3rem;
  background-color: #cfcccc;
}

.home {
  h1 {
    text-align: center;
  }
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  input {
    font-size: 1.1rem;
    outline: none;
    &.item {
      // width: 80%;
    }
    &.price {
      width: 80px;
    }
  }
  ul {
    // width: 90%;
    margin: 10px auto;
    padding: 0;
    li {
      display: flex;
      margin: 2px auto;
      padding: 5px;
      label {
        width: 80px;
        color: grey;
      }
      &.actions {
        button {
          margin-right: 5px;
        }
      }
    }
  }
  form {
    margin: 15px auto;
    background-color: #fff;
    border-radius: 7px;
    padding: 7px;
    ul {
      // width: 90%;
      list-style-type: none;
      li {
        display: flex;
        width: 100%;
      }
      .actions {
        display: flex;
      }
    }
    &.login {

      input {
        width: 60%;
        outline: none;
        border: 1px solid #999;
        &:hover, &:focus {
          background-color: #1172f02a;
        }

        
      }

    }
  }
  .loggedIn {
    width: 95%;
    .logout {
      margin:0 auto;
      display:block;
    }

    div {
      display: flex;
      flex-direction: column;
      width: 100%;
      ul {
        display: flex;
        flex-direction: column;
        list-style-type: none;
        padding: 5px;
        margin: 5px auto;
        background-color: #fff;
        width: 100%;
        justify-content: space-around;
        border-radius: 7px;
        li {
          display: flex;
          // flex-direction: column;
          width: 100%;
          margin: 2px auto;
          // justify-content: space-between;
          @media (min-width: 600px) {
            flex-direction: row;
          }
          label {
            width: 50px;
          }
          span.material-design-icon {
            // color: red;
            display: block;
            // background-color: rgb(228, 190, 190);
            text-align: right;
            float: right;
          }
        }
      }

    }
      section  {
        margin-top: 10px;
        ul {
          color: white;
          background-color: rgb(90, 161, 226);
          label {
            color: white;
          }
        }
      }
      .hidden {
        display: none;
      }      
      li span.material-design-icon {
        padding-left: 10px;
        color: #666;
        &:hover {
          cursor: pointer;
          color: #000;
        }
      }

    @media (min-width: 600px) {
      width: 500px;
    }

  }

  .hours {
    li {
      display: flex;
        justify-content: space-between;
      span {
        display: inline-block;
        width: 50px;
      }
      div {
        display: flex;
        
        input {
          width: 60px;
        }
      }
    }
  }
}
</style>
