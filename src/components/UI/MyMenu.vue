<script>
export default {
  name: 'my-burger-menu',
}
</script>

<template>

  <div class="burger">
    <span class="burger_line burger_line_first"></span>
    <span class="burger_line burger_line_second"></span>
    <span class="burger_line burger_line_third"></span>
  </div>
</template>

<style scoped>
.burger{
  width: 40px;
  height: 28px;
  position: relative;
}
.burger_line{
  display: block;
  width: 100%;
  height: 2px;
  background-color: white;
  position: absolute;
  left: 0;
}
.burger_line_first{
  top:0;
}
.burger_line_second{
  top: 50%;
  transform: translateY(-50%);
}

.burger_line_third{
  bottom: 0;
}
</style>