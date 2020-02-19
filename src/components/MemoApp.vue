<template>
  <div class="memo-app">
    <memo-form @addMemo="addMemo" />
    <ul class="memo-list">
      <memo
        v-for="memo in memos"
        :key="memo.id"
        :memo="memo"
        @deleteMemo="deleteMemo"
        @updateMemo="updateMemo"
        @setEditingId="SET_EDITING_ID"
        @resetEditingId="RESET_EDITING_ID"
        :editingId="editingId"
      />
    </ul>
  </div>
</template>

<script>
import axios from "axios";
import MemoForm from "./MemoForm";
import Memo from "./Memo";

import { mapActions, mapState, mapMutations } from "vuex";
import { RESET_EDITING_ID, SET_EDITING_ID } from "../store/mutations-types";
export default {
  name: "MemoApp",
  components: {
    MemoForm,
    Memo
  },
  computed: { ...mapState(["memos", "editingId"]) },
  created() {
    this.fetchMemos();
  },
  methods: {
    addMemo(payload) {
      memoAPICore.post("/", payload).then(res => {
        this.memos.push(res.data);
      });
      this.$emit("change", this.memos.length);
    },
    deleteMemo(id) {
      const targetIndex = this.memos.findIndex(v => v.id === id);
      memoAPICore.delete(`/${id}`).then(() => {
        this.memos.splice(targetIndex, 1);
      });
      this.$emit("change", this.memos.length);
    },
    storeMemo() {
      const memoToString = JSON.stringify(this.memos);
      localStorage.setItem("memos", memoToString);
    },
    updateMemo(payload) {
      const { id, content } = payload;
      const targetIndex = this.memos.findIndex(v => v.id === id);
      const targetMemo = this.memos[targetIndex];
      memoAPICore.put(`/${id}`, { content }).then(() => {
        this.memos.splice(targetIndex, 1, { ...targetMemo, content });
      });
    },
    ...mapActions(["fetchMemos", "addMemo", "deleteMemo", "updateMemo"]),
    ...mapMutations([SET_EDITING_ID, RESET_EDITING_ID])
  }
};
const memoAPICore = axios.create({
  baseURL: "http://localhost:8000/api/memos"
});
</script>

<style scoped>
.memo-list {
  padding: 20px 0;
  margin: 0;
}
</style>
