<template>
  <div class="w-screen h-screen bg-gray-700">
    <video class="m-auto" :src="slideshowURL" controls="true" autoplay="true"/>
  </div>
</template>

<script>
export default {
  data(){
    return {
      width:900,
      height:500,
      duration:60,
      slideDuration:12000,
      transitionDuration: 1000,
      transitionName:"linearblur",
      format:"auto",
      quality:"auto",
      videoName:"slideshow",
      videoFormat:"mp4",
      assets:[
        {
          publicId:"nuxtjsvideoslideshows/nature/video",
          type:"video"
        },
        {
          publicId:"nuxtjsvideoslideshows/nature/image",
          type:"image"
        },
        {
          publicId:"nuxtjsvideoslideshows/nature/video1",
          type:"video"
        },
        {
          publicId:"nuxtjsvideoslideshows/nature/image1",
          type:"image"
        },
      ]
    }
  },
  computed:{
    slideshowURL(){
      // URL prefix (domain)
      let url =  "https://res.cloudinary.com/";
      
      // Cloud name
      url += process.env.NUXT_ENV_CLOUDINARY_CLOUD_NAME;
      
      // URL prefix completion
      url += "/video/upload/fn_render:";
      
      // Append width
      url += "w_" + this.width + ";";

      // Append height
      url += "h_" + this.height + ";";
      
      // Append duration
      url += "du_"+ this.duration +";";
      
      // Open variables section
      url += "vars_(";
      
      // Append slide duration
      url += "sdur_"+ this.slideDuration +";";
      
      // Append transition duration
      url += "tdur_" + this.transitionDuration + ";";
      
      // Append transition name
      url += "transition_s:" + this.transitionName + ";";
      
      // Open slides section
      url += "slides_(";

      for(let asset of this.assets){
        url += this.transformAsset(asset);
      }
      
      // Close slides section
      url += ")";
      
      // Close variables section
      url += ")";
      
      // Append format and quality setting
      url +="/f_" + this.format + ",q_" + this.quality + "/";
      
      // Append slideshow video name and format
      url += this.videoName + "." + this.videoFormat;

      return url;
    }
  },
  methods:{
      /* 
        Method to transfrom the asset for inclusion in the slideshow URL
        return statement simular to "(media_i:docs:shoppable_sunglasses);"
      */
    transformAsset(asset){
      // Open string
      let assetString = "(media_";

      // Append media type
      assetString += asset.type === "image" ? "i:" : "v:";

      // Append asset id with "/" replaced with ":"
      assetString += asset.publicId.replace(/\//g, ":");

      // If video, append type string
      if(asset.type === "video"){
        assetString += ";type_s:video";
      }

      // Close and return string
      assetString += ");";
      return assetString
    }
  }
}
</script>
