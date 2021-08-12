<template>
  <div class="text-center">
     <div style="padding-top:10px;padding-bottom:10px;">
        <span  style="margin-left:5px;margin-right:5px;">
            <span><b>Category: </b></span>
            <span>{{ this.$props.category }}</span>
        </span>
        <span> / </span>
        <span style="margin-left:5px;margin-right:5px;">
            <span><b>Level: </b></span>
            <span>{{this.$props.difficulty}}</span>
        </span>
     </div>
    <div class="questionblock">
        <transition enter-active-class="animated fadeIn" leave-active-class="animated fadeOut" mode="out-in">
        <span>
            {{ this.$props.question }}
        </span>
        </transition>
    </div>
  </div>
</template>

<script>
import 'animate.css/animate.css';
export default {
props:['category','difficulty','question'],
}
</script>

<style>


</style>