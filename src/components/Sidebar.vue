<template>
  <div class="sidebar">
    <a href="https://www.facebook.com/lilian.soler.5" alt="facebook" aria-label="facebook" class="facebook"
    ><i class="fa-brands fa-facebook"></i>
    </a>
    <a href="https://twitter.com/LilianSoler6" alt="twitter" aria-label="twitter" class="twitter"
    ><i class="fa-brands fa-twitter"></i
    ></a>
    <a href="https://www.instagram.com/lilian64160/" alt="instagram" aria-label="instagram" class="instagram"
    ><i class="fa-brands fa-instagram"></i
    ></a>
    <a
        href="https://www.linkedin.com/in/soler-lilian-2aa832182/" alt="linkedin" aria-label="linkedin"
        class="linkedin"
    ><i class="fa-brands fa-linkedin"></i
    ></a>
    <a
        href="https://www.youtube.com/channel/UCYA1jgZSSP9oISr1nkvIwHw?view_as=subscriber" alt="youtube"
        aria-label="youtube"
        class="youtube"
    ><i class="fa-brands fa-youtube"></i
    ></a>
    <a href="https://www.github.com/LilianSOLER" class="facebook" alt="github" aria-label="github"
    ><i class="fa-brands fa-github"></i
    ></a>
  </div>
</template>

<script lang="ts">
import {defineComponent} from "vue";

export default defineComponent({
  name: "Sidebar",
});
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="scss">
/* Fixed/sticky icon bar (vertically aligned 50% from the top of the screen) */
.sidebar {
  position: fixed;
  top: 50%;
  -webkit-transform: translateY(-50%);
  -ms-transform: translateY(-50%);
  transform: translateY(-50%);

  a {
    display: block;
    text-align: center;
    padding: 1vw;
    transition: all 0.3s ease;
    color: white;
    font-size: 20px;

    :hover {
      background: #000;
    }
  }

  .facebook {
    background: #3b5998;
    color: white;
  }

  .twitter {
    background: #55acee;
    color: white;
  }

  .instagram {
    background: #d6249f;
    background: radial-gradient(
            circle at 30% 107%,
            #fdf497 0%,
            #fdf497 5%,
            #fd5949 45%,
            #d6249f 60%,
            #285aeb 90%
    );
    color: white;
  }

  .linkedin {
    background: #007bb5;
    color: white;
  }

  .youtube {
    background: #bb0000;
    color: white;
  }
}
</style>
