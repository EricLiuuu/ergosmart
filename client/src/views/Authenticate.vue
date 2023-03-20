<script>
import { login, register } from "../service/user";
import { ElMessage } from 'element-plus'
export default {
  data() {
    return {
      isRegister: false
    }
  },
  methods: {
    userRegister({ email, password, role }) {
      register({ email, password, role })
        .then((res) => {
          if (res.code === 0) {
            localStorage.setItem("user", JSON.stringify(res.result));
            this.$router.push('/home')
          } else {
            ElMessage({
              type: 'error',
              message: 'user found',
            })
          }
        });
    },
    userLogin({ email, password }) {
      login({ email, password })
        .then((res) => {
          if (res.code === 0) {
            localStorage.setItem("user", JSON.stringify(res.result));
            this.$router.push('/home')
          } else {
            ElMessage({
              type: 'error',
              message: 'user not found',
            })
          }
        });
    },
    handleSubmit(e) {
      e.preventDefault();
      // validateForm(e);
      const form = e.target;
      const email = form["email"].value;
      const password = form["password"].value;
      if (this.isRegister) {
        const checked = form["role"].checked
        this.userRegister({ email, password, role: checked ? 'ERGONOMIST' : 'USER' });
      } else {
        this.userLogin({ email, password });
      }
    }
  }
}
</script>

<template>
  <header>
    <span class="title">ErgoSmart</span>
  </header>
  <div class="wrapper">
    <form class="login" v-on:submit="handleSubmit">
      <p v-if="isRegister" class="title">Sign up</p>
      <p v-else class="title">Log in</p>

      <input name="email" type="text" placeholder="Email" autofocus />
      <i class="fa fa-envelope"></i>
      <input name="password" type="password" placeholder="Password" />
      <i class="fa fa-key"></i>
      <template v-if="isRegister">
        <!-- svelte-ignore a11y-invalid-attribute -->
        <div>
          <input name="role" id="role" type="checkbox" />
          <label for="role">Are you an Ergonomist?</label>
        </div>
        <a v-on:click="isRegister = false">Already have an account?</a>
      </template>
      <template v-else>
        <!-- svelte-ignore a11y-invalid-attribute -->
        <a v-on:click="isRegister = true">Need Register?</a>
      </template>
      <button type="submit">
        <span class="state">Submit</span>
      </button>
    </form>
  </div>
</template>

<style scoped>
* {
  box-sizing: border-box;
}
header {
  width: 100%;
  height: 70px;
  background-color: #343a40;
  box-sizing: border-box;
  padding: 0 56px;
  display: flex;
  justify-content: space-between;
  align-items: center;
}
.wrapper {
  display: flex;
  align-items: center;
  flex-direction: column;
  justify-content: center;
  width: 100%;
  min-height: 100%;
  padding: 20px;
  background: rgba(4, 40, 68, 0.85);
}

.login {
  border-radius: 2px 2px 5px 5px;
  padding: 10px 20px 20px 20px;
  width: 90%;
  max-width: 320px;
  background: #ffffff;
  position: relative;
  padding-bottom: 80px;
  box-shadow: 0px 1px 5px rgba(0, 0, 0, 0.3);
}

.login input:not([type="checkbox"]) {
  display: block;
  padding: 15px 10px;
  margin-bottom: 10px;
  width: 100%;
  border: 1px solid #ddd;
  transition: border-width 0.2s ease;
  border-radius: 2px;
  color: #ccc;
}

.login input[type="checkbox"] {
  margin-bottom: 10px;
}

.login input+i.fa {
  color: #fff;
  font-size: 1em;
  position: absolute;
  margin-top: -47px;
  opacity: 0;
  left: 0;
  transition: all 0.1s ease-in;
}

.login input:focus {
  outline: none;
  color: #444;
  border-color: #2196f3;
  border-left-width: 35px;
}

.login input:focus+i.fa {
  opacity: 1;
  left: 30px;
  transition: all 0.25s ease-out;
}

.login a {
  font-size: 0.8em;
  color: #2196f3;
  text-decoration: none;
  cursor: pointer;
}

.login .title {
  color: #444;
  font-size: 1.2em;
  font-weight: bold;
  margin: 10px 0 30px 0;
  border-bottom: 1px solid #eee;
  padding-bottom: 20px;
}

.login button {
  cursor: pointer;
  width: 100%;
  height: 100%;
  padding: 10px 10px;
  background: #2196f3;
  color: #fff;
  display: block;
  border: none;
  margin-top: 20px;
  position: absolute;
  left: 0;
  bottom: 0;
  max-height: 60px;
  border: 0px solid rgba(0, 0, 0, 0.1);
  border-radius: 0 0 2px 2px;
  transform: rotateZ(0deg);
  transition: all 0.1s ease-out;
  border-bottom-width: 7px;
}

.login:not(.loading) button:hover {
  box-shadow: 0px 1px 3px #2196f3;
}

.login:not(.loading) button:focus {
  border-bottom-width: 4px;
}

.title {
  color: rgba(255, 255, 255, 0.5);
  font-size: 25px;
}
</style>
