<template>
  <div class="wrapper">
    <parallax class="section page-header header-filter" :style="headerStyle"></parallax>
    <div class="main main-raised" style="z-index:1">
      <div class="section profile-content">
        <div style="padding:80px">
          <h2 class="title text-center kor">게시판 글수정</h2>
          <hr class="div-hr" />
          <form v-on:submit.prevent="modifyContent">
            <md-field>
              <label>제목</label>
              <md-input id="title" type="text" ref="title" v-model="value.title"></md-input>
            </md-field>
            <md-field>
              <label for="category">글 종류</label>
              <md-select v-model="category" name="category" id="category">
                <md-option value="공지사항">공지사항</md-option>
                <md-option value="일반">일반</md-option>
                <md-option value="QnA">QnA</md-option>
              </md-select>
            </md-field>
            <md-field>
              <div id="summernote"></div>
            </md-field>
            <md-field>
              <!-- 파일의 경우 change 리스너로 감지해야함 -->
              <input type="file" name="uploadFile" ref="fileData" />
              <!-- <input type="file" name="uploadFile" ref="fileData" @change="handleFilesUpload" /> -->
            </md-field>
          </form>

          <div class="btn-right">
            <v-btn color="rgb(33,133,89)" class="mr-1" dark type="submit" @click="modifyBoard()">
              수정완료
            </v-btn>
            <v-btn type="button" color="rgb(68, 114, 148)" dark @click="moveBoard">
              뒤로가기
            </v-btn>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import { detailBoard, editBoard } from '@/api/board';

export default {
  bodyClass: 'profile-page',
  data: function() {
    return {
      value: '',
      content: '',
      category: '',
    };
  },
  props: {
    header: {
      type: String,
      default: require('@/assets/img/bg_board.jpg'),
    },
  },
  computed: {
    headerStyle() {
      return {
        backgroundImage: `url(${this.header})`,
      };
    },
  },
  async created() {
    const postData = this.$route.query.boardId;
    const { data } = await detailBoard(postData);
    this.value = data;
    $(function() {
      $('#summernote').summernote({
        height: 300, // set editor height
        width: '100%', // set editor weight
        minHeight: null, // set minimum height of editor
        maxHeight: null, // set maximum height of editor
        dialogsInBody: true,
        toolbar: [
          ['style', ['style']],
          ['font', ['bold', 'italic', 'underline', 'clear']],
          ['fontname', ['fontname']],
          ['color', ['color']],
          ['para', ['ul', 'ol', 'paragraph']],
          ['height', ['height']],
          ['table', ['table']],
          ['insert', ['media', 'link', 'hr', 'picture', 'video']],
          ['view', ['fullscreen', 'codeview']],
        ],
      });
      $('#summernote').summernote('pasteHTML', data.content);
    });
  },
  methods: {
    async modifyBoard() {
      const editData = {
        boardId: this.value.boardId,
        category: this.value.category,
        content: $('#summernote').summernote('code'),
        title: this.value.title,
        name: this.value.name,
        uuid: this.value.uuid,
      };

      const role = this.$store.state.role;
      const { data } = await editBoard(editData, role);

      this.$swal({
        icon: 'success',
        title: '글 수정 완료',
      });
      var query = this.value.boardId;
      this.$router.push({ name: 'boardDetail', query: { boardId: query } });
    },
    moveBoard() {
      this.$router.push({ name: 'board' });
    },
  },
};
</script>

<style lang="scss" scoped>
// hr 설정
.div-hr {
  width: 80%;
}
// 한글 폰트 설정
.kor {
  font-family: 'Nanum Gothic', sans-serif;
}
// table css
.styled-table {
  border-collapse: collapse;
  margin: 25px 0;
  font-size: 0.9em;
  font-family: sans-serif;
  min-width: 400px;
  box-shadow: 0 0 20px rgba(0, 0, 0, 0.15);
}
.styled-table th {
  background-color: #98cec3;
  color: #ffffff;
  text-align: center;
  width: 10rem;
}
.styled-table th,
.styled-table td {
  padding: 12px 15px;
}
</style>
