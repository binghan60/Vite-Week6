<template>
  <h2>這是後台</h2>
  <nav>
    <RouterLink to="/admin/products">產品列表</RouterLink> |
    <RouterLink to="/admin/order">訂單列表</RouterLink> |
    <RouterLink to="/">返回前台</RouterLink> |
  </nav>
  <RouterView></RouterView>
</template>

<script>
import axios from 'axios';

const { VITE_APP_URL } = import.meta.env;
export default {
  methods: {
    checkUser() {
      axios.post(`${VITE_APP_URL}/api/user/check`)
        .then((res) => {
          console.log(res.data.success);
        })
        .catch(() => {
          this.$router.push('/login');
        });
    },
  },
  mounted() {
    const token = document.cookie.replace(/(?:(?:^|.*;\s*)hexVueToken\s*=\s*([^;]*).*$)|^.*$/, '$1');
    axios.defaults.headers.common.Authorization = token;
    this.checkUser();
  },
};
</script>
