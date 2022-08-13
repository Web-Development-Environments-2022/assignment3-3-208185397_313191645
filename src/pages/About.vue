<template>
  <div class="container"> <br>
    <h1 class="title">About</h1>
    <div>
    <b-carousel
      id="carousel-1"
      v-model="slide"
      :interval="4000"
      controls
      indicators
      background="#ababab"
      img-width="1200"
      img-height="700"
      style="text-shadow: 1px 1px 2px #333;"
      @sliding-start="onSlideStart"
      @sliding-end="onSlideEnd"
    >
      <!-- Text slides with image -->
      <b-carousel-slide class="carousel"
        caption="BGU Students"
        text="As part as out jurney to become information system's engineers at Ben Gurion University, We took the course Web development enviorments."
        img-src="https://www.abidat.in/images/Ben-Gurion-University-of-the-Negev.jpg"
      ></b-carousel-slide>

    <b-carousel-slide
        caption="Programmers"
        text="We learned web development, including tech like JS, JQuery, Node.js, Express.js and Vue.js"
        img-src="https://www.stuffroots.com/wp-content/uploads/2022/01/How-Can-College-Students-Get-Coding-Assignment-Help.jpg"
      ></b-carousel-slide>
    <b-carousel-slide
        caption="Senior Citizen Friendly"
        text="We started by developing a static web site for our senior relatives, to pass their time during the COVID pandemic"
        img-src="https://www.leinsterexpress.ie/resizer/1200/700/true/1540912277336.jpg--some_laois_day_care_for_elderly_to_be_restored_in_2022.jpg?1540912277000"
      ></b-carousel-slide>

      <b-carousel-slide 
        img-src="https://selecciones.com.mx/wp-content/uploads/2017/01/Pac-Man.jpg"
        caption="HTML 5 Know-How's"
        text="We also developed a PAC-Man game using HTML5 canvas tech"      
      ></b-carousel-slide>

      <b-carousel-slide 
        img-src="https://savvy.co.il/wp-content/uploads/2021/02/disable-wp-json-rest-api-in-wordpress.jpg"
        caption="Backend Experience"
        text="We Proceeded to developing a backend API service, which we are currently using for the web site you are browsing!"      
      ></b-carousel-slide>
      
      <b-carousel-slide 
        img-src="https://www.goodtoseo.com/wp-content/uploads/2017/09/getty_91697575_297616-1200x700.jpg"
        caption="Full Stack"
        text="This web site is developed by us, we are using Vue.js for UI and Node + Express js for the backend side. It uses an internal DB but also an outside API for the data you are browsing."
      ></b-carousel-slide>
            
    </b-carousel>
  </div>
  <div>
    <b-list-group>  
        <b-list-group-item><a class="links" href="https://web-development-environments-2022.github.io/313191645/" target=”_blank”>Ben's site (Senior Citizen work)</a></b-list-group-item>
        <b-list-group-item><a class="links" href="https://web-development-environments-2022.github.io/208185397/" target=”_blank”>Ilan's site (Senior Citizen work)</a></b-list-group-item>
        <b-list-group-item><a class="links" href="https://web-development-environments-2022.github.io/assignment2-313191645_208185397/" target=”_blank”>PAC-Man game in HTML5</a></b-list-group-item>
        <b-list-group-item><a class="links" href="https://app.swaggerhub.com/apis/BenAidlin/RecepieBookAPI/1.0.0" target=”_blank”>Web API documentation in swagger.io</a></b-list-group-item>
    </b-list-group>
    <br><br><br><br>
  </div>
  </div>
</template>

<script>

export default {
  components: {

  },
  data() {
      return {
        slide: 0,
        sliding: null
      }
    },
    methods: {
      onSlideStart(slide) {
        this.sliding = true
      },
      onSlideEnd(slide) {
        this.sliding = false
      }
    }
};
</script>

<style lang="scss" scoped>
.title{
  font-family:Impact, Haettenschweiler, 'Arial Narrow Bold', sans-serif;
}
.carousel{
    color: black !important;
}
.links{
    color: darkslategrey;
    font-weight:900;
}
</style>
