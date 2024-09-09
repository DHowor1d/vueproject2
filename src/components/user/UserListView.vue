<template>
  <div
    v-for="(item, i) in $store.state.userData"
    :key="i"
    class="d-flex gap-2 border rounded p-3 mb-3"
  >
    <div class="avata">
      <img :src="item.image" alt="" />
    </div>
    <div class="d-flex flex-grow-1 justify-content-between align-items-center">
      <div>
        <h5 class="mb-0">{{ item.userid }}</h5>
        <p class="mb-0">{{ item.username }}</p>
      </div>
      <button class = "detail" @click="openModal(item)">상세보기</button>
      <button class = "delete" @click="openModal(item)">삭제</button>
    </div>
  </div>
  <UserDetailModal 
    :show="isModalOpen" 
    :user="selectedUser" 
    @close="closeModal" 
    @updateUser="updateUser"
  />
</template>

<script setup>
import { ref } from 'vue';
import { useStore } from 'vuex';
import UserDetailModal from './UserDetailView.vue';

const store = useStore();
const isModalOpen = ref(false);
const selectedUser = ref(null);

const openModal = (user) => {
  selectedUser.value = user;
  isModalOpen.value = true;
};

const closeModal = () => {
  isModalOpen.value = false;
};

const updateUser = (updatedUser) => {
  console.log('updateUser 호출됨', updatedUser); // 콘솔 로그 추가
  // 사용자 데이터를 업데이트하는 로직을 추가합니다.
  const index = store.state.userData.findIndex(user => user.userid === updatedUser.userid);
  if (index !== -1) {
    store.commit('updateUser', { index, updatedUser });
  }
  closeModal();
};
</script>

<style scoped>
.avata {
  width: 50px;
  height: 50px;
  border-radius: 50%;
  overflow: hidden;
}

.detail {
  background-color: #007bff;
  color: white;
  border: none;
  padding: 10px 20px;
  border-radius: 5px;
  cursor: pointer;
  transition: background-color 0.3s;
}

.detail:hover {
  background-color: #0056b3;
}

.delete {
  background-color: #f94c4c;
  color: white;
  border: none;
  padding: 10px 20px;
  border-radius: 5px;
  cursor: pointer;
  transition: background-color 0.3s;
}

.delete:hover {
  background-color: hsl(0, 100%, 51%);
}

</style>

