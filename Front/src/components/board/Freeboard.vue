<template>
<div>  
  <div class="post-wrap"  v-for="post in freePostList" :key="post.id">
    <!--  v-for="post in freepost" :key="post.id" -->
    <div class="d-flex justify-content-between align-items-center">
      <div class="d-flex align-items-center">
        <div id="img-circle" class="d-inline-block">
          <b-img :src="post.profile" class="img-fluid"></b-img>
          <!-- <img src="https://picsum.photos/48/48" alt="프로필사진"> -->
        </div>      
        <span class="d-inline-block ms-2" id="post-username">
          {{ post.nickname }}
        </span>        
      </div>        
      <div class="d-inline-block">
        <span id="post-time">
        {{ post.created_time.date.year }}.
        {{ post.created_time.date.month }}.
        {{ post.created_time.date.day }}
        </span>
      </div>      
    </div>

    <!-- 글 -->
    <router-link
      :to="{ name: 'FreeboardDetail', params: { id: post.id }}"
      class="text-decoration-none text-dark m-0 p-0"
    >    
      <div class="content-wrap my-2">
        <div class="text-start mt-1" id="post-title">
          <!-- 제목 -->
          {{ post.title }}
        </div>
        <div class="text-start mt-1 mb-2" id="post-content">
          {{ post.contents }}        
        </div>
        <div class="post-img" v-if="post.image">              
          <b-img :src="post.image" class="img-fluid" alt="이미지"></b-img>
          <!-- <img src="https://picsum.photos/392/180" alt="이미지"> -->
        </div>
        <div v-else></div>
      </div>
    </router-link>

    <!-- 좋아요, 리플 수 -->
    <div class="text-end" id="post-upcount">
      <div class="d-inline-block me-3">
        <b-icon icon="heart-fill" aria-hidden="false" class="me-2"></b-icon>
        <span>{{ post.upCount }}</span>
      </div>
      <div class="d-inline-block">
        <b-icon icon="chat-left" aria-hidden="false" class="me-2"></b-icon>
        <span>{{ post.commentCount }}</span>
      </div>
    </div>
  </div> 
</div>
</template>

<script>
import http from "@/util/http-common";

export default {
  name: "Freeboard",
  components: {
  },
  data () {
    return {
      freePostList: [],
      username: '',
    };
  },
  methods: {
    getFreePostList: function () {
      http.get("board/")
      .then(response => {
        // console.log(response.data)
        this.freePostList = response.data
        // this.username = this.$store.state.user.login_id
      }).catch(err => {
        console.log(err)
      });
    },
  },
  created: function () {
    this.getFreePostList()
  }
}
</script>

<style scoped>
  .post-wrap {
    padding: 12px 14px 12px 14px;
    border-bottom: solid 1px #eee;
    color: #444;
  }

  .content-wrap {
    overflow: hidden;
  }

  .post-img {
    max-height: 400px;
  }
</style>