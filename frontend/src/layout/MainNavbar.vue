<template>
  <md-toolbar id="toolbar" md-elevation="0" class="md-transparent md-absolute kor" :class="extraNavClasses" :color-on-scroll="colorOnScroll">
    <div class="md-toolbar-row md-collapse-lateral">
      <div class="md-toolbar-section-start">
        <h3 class="md-title">
          <router-link to="/" class="md-title kor">
            <i class="material-icons">gavel</i>
            <!-- 法대로 합시다 -->
            법대로 합시다
          </router-link>
        </h3>
      </div>
      <div class="md-toolbar-section-end">
        <md-button class="md-just-icon md-simple md-toolbar-toggle" :class="{ toggled: toggledClass }" @click="toggleNavbarMobile()">
          <span class="icon-bar"></span>
          <span class="icon-bar"></span>
          <span class="icon-bar"></span>
        </md-button>

        <div class="md-collapse">
          <div class="md-collapse-wrapper">
            <mobile-menu nav-mobile-section-start="false"></mobile-menu>
            <md-list>
              <li class="md-list-item">
                <router-link to="/lawyermatch" class="md-list-item-router md-list-item-container md-button-clean">
                  <!-- <router-link to="/adviseList" class="md-list-item-router md-list-item-container md-button-clean"> -->
                  <div class="md-list-item-content">
                    <md-button slot="title" class="md-button md-button-link md-white md-simple">
                      <i class="material-icons">assignment_turned_in</i>
                      변호사자문매칭
                    </md-button>
                  </div>
                </router-link>
              </li>

              <li class="md-list-item">
                <router-link to="" class="md-list-item-router md-list-item-container md-button-clean">
                  <!-- <router-link to="/adviseList" class="md-list-item-router md-list-item-container md-button-clean"> -->
                  <div class="md-list-item-content">
                    <md-button slot="title" class="md-button md-button-link md-white md-simple">
                      <a @click="moveScourtExp" target="_blank">
                        <i class="material-icons">gavel</i>
                        모의판사체험
                      </a>
                    </md-button>
                  </div>
                </router-link>
              </li>
              <li class="md-list-item">
                <router-link to="/controversylist" class="md-list-item-router md-list-item-container md-button-clean">
                  <div class="md-list-item-content">
                    <md-button slot="title" class="md-button md-button-link md-white md-simple">
                      <i class="material-icons">supervisor_account</i>
                      찬반토론
                    </md-button>
                  </div>
                </router-link>
              </li>
              <li class="md-list-item">
                <router-link to="/lawtest" class="md-list-item-router md-list-item-container md-button-clean">
                  <div class="md-list-item-content">
                    <md-button slot="title" class="md-button md-button-link md-white md-simple">
                      <i class="material-icons">quiz</i>
                      법상식테스트
                    </md-button>
                  </div>
                </router-link>
              </li>
              <li class="md-list-item">
                <router-link to="/easylaw" class="md-list-item-router md-list-item-container md-button-clean">
                  <div class="md-list-item-content">
                    <md-button slot="title" class="md-button md-button-link md-white md-simple">
                      <i class="material-icons">info</i>
                      생활법령
                    </md-button>
                  </div>
                </router-link>
              </li>

              <li class="md-list-item">
                <router-link to="/board" class="md-list-item-router md-list-item-container md-button-clean">
                  <div class="md-list-item-content">
                    <md-button slot="title" class="md-button md-button-link md-white md-simple">
                      <i class="material-icons">assignment_turned_in</i>
                      게시판
                    </md-button>
                  </div>
                </router-link>
              </li>
              <template v-if="this.$store.state.token">
                <li class="md-list-item">
                  <!-- USER프로필 완성되면 to경로 바꾸깅 -->
                  <router-link :to="this.$store.state.role == 'USER' ? { name: 'profileUser' } : { name: 'profileLawyer' }" class="md-list-item-router md-list-item-container md-button-clean">
                    <div class="md-list-item-content">
                      <md-button slot="title" class="md-button md-button-link md-white md-simple">
                        <i class="material-icons">account_circle</i>
                        {{ $store.state.name }}의 프로필
                      </md-button>
                    </div>
                  </router-link>
                </li>

                <li class="md-list-item">
                  <div class="md-list-item-content">
                    <md-button slot="title" class="md-button md-button-link md-white md-simple" @click="logoutUser()">
                      <i class="material-icons">account_circle</i>
                      로그아웃
                    </md-button>
                  </div>
                </li>
              </template>

              <!-- 비로그인 -->
              <template v-else>
                <!-- 로그인 부분 -->
                <li class="md-list-item">
                  <router-link to="/login" class="md-list-item-router md-list-item-container md-button-clean">
                    <div class="md-list-item-content">
                      <md-button slot="title" class="md-button md-button-link md-white md-simple">
                        <i class="material-icons">account_circle</i>
                        로그인
                      </md-button>
                    </div>
                  </router-link>
                </li>

                <li class="md-list-item">
                  <router-link to="/registerindex" class="md-list-item-router md-list-item-container md-button-clean">
                    <div class="md-list-item-content">
                      <md-button slot="title" class="md-button md-button-link md-white md-simple">
                        <i class="material-icons">person_add</i>
                        회원가입
                      </md-button>
                    </div>
                  </router-link>
                </li>
              </template>
            </md-list>
          </div>
        </div>
      </div>
    </div>
  </md-toolbar>
</template>

<script>
let resizeTimeout;
function resizeThrottler(actualResizeHandler) {
  // ignore resize events as long as an actualResizeHandler execution is in the queue
  if (!resizeTimeout) {
    resizeTimeout = setTimeout(() => {
      resizeTimeout = null;
      actualResizeHandler();

      // The actualResizeHandler will execute at a rate of 15fps
    }, 66);
  }
}

import MobileMenu from '@/layout/MobileMenu';
export default {
  components: {
    MobileMenu,
  },
  props: {
    type: {
      type: String,
      default: 'white',
      validator(value) {
        return ['white', 'default', 'primary', 'danger', 'success', 'warning', 'info'].includes(value);
      },
    },
    colorOnScroll: {
      type: Number,
      default: 0,
    },
  },
  data() {
    return {
      extraNavClasses: '',
      toggledClass: false,
      role: this.$store.state.role,
    };
  },
  computed: {
    showDownload() {
      const excludedRoutes = ['login', 'landing', 'profile'];
      return excludedRoutes.every((r) => r !== this.$route.name);
    },
  },
  methods: {
    moveScourtExp() {
      window.open('about:blank').location.href = 'https://i4d103.p.ssafy.io/exp/main.html';
    },
    logoutUser() {
      this.$store.commit('clearEmail');
      this.$store.commit('clearToken');
      this.$store.commit('clearNickname');
      this.$store.commit('clearName');
      this.$store.commit('clearPassword');
      this.$store.commit('clearUuid');
      // this.$store.commit('clearImage');
      this.$store.commit('clearLawuuid');
      localStorage.clear();
      sessionStorage.clear();
      $cookies.keys().forEach((cookie) => $cookies.remove(cookie));
      this.$router.push({ name: 'login' });
    },
    bodyClick() {
      let bodyClick = document.getElementById('bodyClick');

      if (bodyClick === null) {
        let body = document.querySelector('body');
        let elem = document.createElement('div');
        elem.setAttribute('id', 'bodyClick');
        body.appendChild(elem);

        let bodyClick = document.getElementById('bodyClick');
        bodyClick.addEventListener('click', this.toggleNavbarMobile);
      } else {
        bodyClick.remove();
      }
    },
    toggleNavbarMobile() {
      this.NavbarStore.showNavbar = !this.NavbarStore.showNavbar;
      this.toggledClass = !this.toggledClass;
      this.bodyClick();
    },
    handleScroll() {
      let scrollValue = document.body.scrollTop || document.documentElement.scrollTop;
      let navbarColor = document.getElementById('toolbar');
      this.currentScrollValue = scrollValue;
      if (this.colorOnScroll > 0 && scrollValue > this.colorOnScroll) {
        this.extraNavClasses = `md-${this.type}`;
        navbarColor.classList.remove('md-transparent');
      } else {
        if (this.extraNavClasses) {
          this.extraNavClasses = '';
          navbarColor.classList.add('md-transparent');
        }
      }
    },
    scrollListener() {
      resizeThrottler(this.handleScroll);
    },
    scrollToElement() {
      let element_id = document.getElementById('downloadSection');
      if (element_id) {
        element_id.scrollIntoView({ block: 'end', behavior: 'smooth' });
      }
    },
  },
  mounted() {
    document.addEventListener('scroll', this.scrollListener);
  },
  beforeDestroy() {
    document.removeEventListener('scroll', this.scrollListener);
  },
};
</script>

<style lang="scss" scoped>
.kor {
  font-family: 'cwTeXHei', 'S-CoreDream-6Bold'; // 한자폰트 + 한글폰트
}
</style>
