<template>
  <div id='login'>
    <form>
      <div class='loginFormTab'>
        <div @click='FormStatus = true' :class='{ active: FormStatus }'>
          登 入
        </div>
        <div @click='FormStatus = false' :class='{ active: !FormStatus }'>
          註 冊
        </div>
      </div>
      <div class='loginFromContainer'>
        <div class='loginForm'>
          <div>
            <i class='fa-solid fa-user'></i>
            <input v-model='username' type='text' name='username' placeholder='請輸入帳號' />
          </div>
          <div>
            <i class='fa-solid fa-lock'></i>
            <input v-model='password' type='password' name='password' placeholder='請輸入密碼' />
          </div>
          <button v-if='FormStatus' type='button' @click='login()'>
            登 入
          </button>
          <button v-else type='button' @click='register()'>註 冊</button>
        </div>
      </div>
    </form>
  </div>
</template>

<script>
import axios from 'axios';

const { VITE_APP_URL } = import.meta.env;

export default {
  data() {
    return {
      username: '',
      password: '',
      FormStatus: true,
    };
  },
  methods: {
    login() {
      const { username, password } = this;
      if (!username.trim()) {
        this.username = '';
        document.querySelector('input[name=username]').focus();
        alert('請輸入帳號');
        return;
      }
      if (!password.trim()) {
        this.password = '';
        document.querySelector('input[name=password]').focus();
        alert('請輸入密碼');
        return;
      }
      const user = {
        username,
        password,
      };
      axios
        .post(`${VITE_APP_URL}/admin/signin`, user)
        .then((res) => {
          const { success, token, expired } = res.data;
          if (success) {
            document.cookie = `hexVueToken=${token}; expires=${new Date(expired)}`;
            this.$router.push('./admin/products');
          }
        })
        .catch((err) => {
          alert(err.response.data.message);
        });
    },
    register() {
      alert('尚未開放註冊');
    },
  },
  mounted() { },
};
</script>

<style scope>
#login {
  background: url(./images/OIG.IJ.jpg) center/cover no-repeat;
  width: 100%;
  height: 100vh;
  display: flex;
  justify-content: center;
  align-items: center;
}

#login Form {
  height: 80%;
  max-height: 500px;
  min-height: 350px;
  width: 300px;
  background-color: rgba(78, 78, 78, 0.9);
  transition: 0.5s;
}

.loginFormTab {
  display: flex;
}

.loginFormTab div {
  text-align: center;
  line-height: 40px;
  height: 40px;
  background-color: #fff;
  width: 50%;
}

.loginFormTab .active {
  background-color: #44c8ba;
  font-weight: 900;
}

.loginFromContainer {
  display: flex;
  align-items: center;
  height: 90%;
}

.loginForm {
  width: 100%;
  display: flex;
  flex-direction: column;
  align-items: center;
  padding: 15px;
}

.loginForm div {
  width: 100%;
}

.loginForm input {
  width: 100%;
  height: 35px;
  background-color: #656565;
  margin: 20px 0;
  border: none;
  box-shadow: 2px 2px 5px 0px rgba(0, 0, 0, 0.75);
  color: #fff;
  text-indent: 25px;
  transition: 0.3s;
}

.loginForm input {
  padding-block: 0px;
  padding-inline: 0px;
}

.loginForm i {
  position: relative;
  font-size: 0.8rem;
  top: 45px;
  left: 10px;
  color: #a5a5a5;
}

.loginForm input::placeholder {
  color: #a5a5a5;
}

.loginForm input:focus {
  outline: 1px solid #44c8ba;
}

.loginForm button {
  margin: 20px 0;
  line-height: 30px;
  height: 30px;
  width: 100px;
  background-color: #44c8ba;
  color: #fff;
  border: none;
  box-shadow: 2px 2px 5px 0px rgba(0, 0, 0, 0.75);
  transition: 0.5s;
  font-weight: 900;
}

@media only screen and (min-width: 768px) {
  #login Form {
    width: 450px;
    max-height: 675px;
    min-height: 450px;
  }

  .loginFormTab div {
    line-height: 50px;
    height: 50px;
    background-color: #fff;
    width: 50%;
  }

  .loginForm {
    padding: 30px;
  }

  .loginForm div {
    width: 100%;
    height: 125px;
  }

  .loginForm i {
    position: relative;
    top: 52px;
    left: 10px;
    font-size: 1rem;
    color: #a5a5a5;
  }

  .loginForm input {
    height: 47px;
    text-indent: 30px;
    font-size: 1rem;
  }

  .loginForm button {
    margin: 30px 0;
    line-height: 45px;
    height: 45px;
    width: 150px;
    font-size: 1.25rem;
    cursor: pointer;
  }
}

/*  */

.loadingContainer {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100vh;
  display: flex;
  align-items: center;
  justify-content: center;
  z-index: 9999;
  background-color: rgba(0, 0, 0, 0.5);
}

.loader {
  border: 12px solid #f3f3f3;
  border-top: 12px solid #3498db;
  border-radius: 50%;
  width: 80px;
  height: 80px;
  animation: spin 1s linear infinite;
}

@keyframes spin {
  0% {
    transform: rotate(0deg);
  }

  100% {
    transform: rotate(360deg);
  }
}

.content {
  padding: 20px;
  background-color: #fff;
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
  border-radius: 8px;
  margin-top: 120px;
}</style>
