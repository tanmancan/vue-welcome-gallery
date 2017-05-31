<template>
  <div class="gallery-container">
    <div class="gallery-item"></div>
    <div class="gallery-item"></div>
    <div class="gallery-item"></div>
  </div>
</template>

<style scoped lang="scss">
  .gallery-container {
    display: flex;
    flex-flow: row;
    width: 100vw;
    height: 100vh;
    opacity: .45;
    .gallery-item {
      flex: 1 1 percentage(1/3);
      transition: opacity 1s ease;
      &:nth-child(1) {
      }
      &:nth-child(2) {
      }
      &:nth-child(3) {
      }
    }
  }
</style>

<script>
  export default {
    props: ['images', 'imgDir'],
    data() {
      return {
        imgArr: this.images.split(',')
      }
    },
    mounted() {
      this.init();
    },
    methods: {
      // Get 3 random images and assign them to each slide
      init() {
        this.raf = window.requestAnimationFrame;
        this.slides = document.querySelectorAll('.gallery-item');

        for (var i = this.slides.length - 1; i >= 0; i--) {
          let slide = this.slides[i],
              currentImg = this.getRandImage();

          slide.currentImg = currentImg;
          slide.animationStart = null;

          this.updateSlideImage(slide, slide.currentImg);
        }

        this.animateSlide(this.slides[0], 3000);
        this.animateSlide(this.slides[1], 5000);
        this.animateSlide(this.slides[2], 7000);
      },
      // Animate slides
      animateSlide(slide, duration = 2000) {
        let timeId = window.setTimeout(() => {
          let img = this.getRandImage(slide.currentImg),
            randDuration = Math.floor(Math.random() * duration),
            durationOffset = (randDuration > 10000) ? randDuration : randDuration + 6000;

          slide.currentImg = img;
          slide.style.opacity = 0;
          // Create a transition
          window.setTimeout(() => {
            this.updateSlideImage(slide, img);
            slide.style.opacity = 1;
          }, 500);
          this.animateSlide(slide, durationOffset);
          console.log('timeout');
        }, duration);
      },
      // Updates the slide image style
      updateSlideImage(slide, img) {
        slide.style.background = `url(${this.imgDir}/${img}) center center no-repeat`;
        slide.style.backgroundSize = 'cover';
      },
      // Gets a random image from the image array
      // If an old image was provided, replace the random image with the old image
      getRandImage(oldImage = null) {
        let newImage = '',
          imgArr = this.imgArr,
          len = this.imgArr.length,
          randKey = Math.floor(Math.random() * len);

        // Get a random image using the random key
        newImage = this.imgArr[randKey];

        if (oldImage) {
          // Create a new array with the random key value replaced with the oldImage
          this.imgArr = imgArr.map((val, idx) => {
            if (randKey === idx) {
              return oldImage;
            }
            return val;
          });
        }else {
          // If oldImage was not provided then remove the random image from the image array
          this.imgArr.splice(randKey, 1);
        }

        return newImage;
      }
    }
  }
</script>
