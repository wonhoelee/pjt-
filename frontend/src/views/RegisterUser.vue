<template>
  <!-- 회원가입 페이지 -->
  <div class="wrapper">
    <parallax class="section page-header header-filter" :style="headerStyle"></parallax>
    <div class="main main-raised">
      <div class="section profile-content">
        <hr />
        <div class="container">
          <register-insert-normal></register-insert-normal>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import RegisterInsertNormal from './components/RegisterInsertNormal.vue';

export default {
  components: { RegisterInsertNormal },
  bodyClass: 'profile-page',
  data() {
    return {};
  },
  props: {
    header: {
      type: String,
      default: require('@/assets/img/bg_signup.jpg'),
    },
    city: {
      type: String,
    },
  },
  computed: {
    headerStyle() {
      return {
        backgroundImage: `url(${this.header})`,
      };
    },
  },
};
</script>

<style lang="scss" scoped>
// 한글 폰트 설정
.kor {
  font-family: 'Nanum Gothic', sans-serif;
}
.section {
  padding: 0;
}
</style>
