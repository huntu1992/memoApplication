<template>
  <div class="app-header">
    <h1>Memo application</h1>
    <p>
      <strong>{{ getMemoCount }}</strong
      >개
    </p>
  </div>
</template>

<script>
import { mapGetters } from "vuex";
export default {
  name: "AppHeader",
  computed: {
    // getMemoCount() {
    //   return this.memos.length;
    // }
    ...mapGetters(["getMemoCount"])
  }
  // props: {
  //   memoCount: {
  //     type: Number,
  //     default: 0
  //   }
  // },
};
</script>

<style scoped>
.app-header {
  overflow: hidden;
  padding: 52px 0 27px;
}
.app-header h1 {
  float: left;
  font-size: 24px;
  text-align: center;
}
.app-header p {
  float: right;
  padding-top: 6px;
  font-size: 18px;
  vertical-align: bottom;
}
.app-header p strong {
  color: #ff5a00;
}
</style>
