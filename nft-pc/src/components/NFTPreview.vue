<template>
  <div class="nft-preview">
    <div class="audio-preview" v-if="previewType=='audio'">
      <el-image :id="audioImgId" class="nft-image fullImage" :src="$filters.fullImageUrl(imageIpfs)" @load="handleResize" >
        <template v-slot:error>
          <el-image class="error-image" :src="require('@/assets/create-img/non-existent.png')" fit="contain"></el-image>
        </template>
      </el-image>
      <div class="audio-inner">
        <audio class="nft-audio" :src="$filters.fullImageUrl(animation_url)" autoplay
          :controls="controls" controlslist="nodownload" loop
          :muted="!preview" preload="none" :style="{marginTop: audioTop + 'px'}"></audio>
      </div>
    </div>
    <div class="video-preview" v-else-if="previewType=='video'">
      <video class="nft-video" :src="$filters.fullImageUrl(animation_url)" autoplay
        :controls="controls" disablePictureInPicture controlslist="nodownload" loop
        :muted="!preview" :poster="$filters.fullImageUrl(imageIpfs)"
        ></video>
    </div>
    <div class="image-preview" v-else>
      <el-image class="nft-image fullImage" :src="$filters.fullImageUrl(imageIpfs)">
        <template v-slot:error>
          <el-image class="error-image" :src="require('@/assets/create-img/non-existent.png')" fit="contain"></el-image>
        </template>
      </el-image>
    </div>

  </div>
</template>
<script>
export default {
  name: "NFTPreview",
  props: {
    imgUrl: {
      type: String,
      default: "",
    },
    animUrl: {
      type: String,
      default: ""
    },
    preview: {
      type: Boolean,
      default: false,
    },
    animation_url:{
      type: String,
      default:"",
    },
    imageIpfs:{
      type: String,
      default:"",
    }
  },
  data(){
    return {
      autoPlay: true,
      controls: true,
      audioWidth: 0,
      audioTop: 0,
      audioImgId: this.preview ? "audio-image-preview" : "audio-image"
    };
  },
  mounted(){
    window.addEventListener("resize", this.handleResize);
  },
  unmounted(){
    window.removeEventListener("resize", this.handleResize);
  },
  computed: {
    previewType(){
      if(!this.imgUrl) return;
      if(!this.animUrl) return "image"
      if(this.$tools.isAudioUrl(this.animUrl)) return 'audio';

      if(this.$tools.isVideoUrl(this.animUrl)) return 'video'
    },
  },
  methods: {
    handleResize(){
      let audioImg = document.getElementById(this.audioImgId);
      if(!audioImg) return;
      let width = audioImg.clientWidth;
      let height = audioImg.clientHeight;
      this.audioWidth = width * 0.9;
      this.audioTop = height * 0.5 - 50;
    },

  }
}
</script>

<style lang="scss" scoped>

.audio-preview{
  padding-bottom: 35px;
}

.nft-preview, .image-preview, .audio-preview, .video-preview, .no-preview{
  width: 100%;
  height: 100%;
  display: flex;
  align-items: center;
  justify-content: center;
  position: relative;
}
.nft-audio{
  width: 100%;
  height: 32px;
  display: block;
}

.nft-image, .nft-video{
  display:flex;
  justify-content: center;
  align-items: center;
  max-width: 100%;
  max-height: 100%;
  border-radius: $borderRadius;
}
.nft-preview{
  .audio-holder{
    width: 100%;
    height: 50%;
  }
  .audio-inner{
    position: absolute;
    width: 100%;
    bottom: 0;
    text-align: center;
  }
}
.error-image{
  width:100%;
  height:100%;
  display: flex;
    justify-content: center;
    align-items: center;
}
</style>
