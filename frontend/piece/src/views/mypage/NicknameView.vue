<template>
    <div class="nicknameview-main-container">
        <div class="nicknameview-main-title">닉네임을 입력해주세요</div>

        <div class="nicknameview-input-label">닉네임</div>
        <input
            class="nicknameview-input-input"
            type="text"
            maxlength="10"
            required
            @input="checkNickname"
            v-model='nickname'
        />
        <div class="nicknameview-input-message">
            {{ nicknameMessage }}
        </div>
    </div>

    <SquareButton
        class="nicknameeview-main-button"
        :squareButtonContent="'확인'"
        :squareButtonFunction="handleNicknameClick"
        :isSquareDisable="isNickname"
    ></SquareButton>

    <!-- modal -->
    <SuccessModal
        v-if="isModal"
        :modalTitle="'닉네임이 변경되었어요!'"
        :handleSuccessClick="handleChangeSuccess"
    />
</template>

<script setup>
import router from "@/router";
import { ref, onMounted, computed } from "vue";
import { useCommonStore } from "@/stores/common";
import { useUserStore } from "@/stores/user";
import SquareButton from "@/components/button/SquareButton.vue";
import SuccessModal from "@/components/modal/SuccessModal.vue";

const commonStore = useCommonStore();
const store = useUserStore();

const nicknameMessage = computed(() => store.getNicknameMessage);
const isNickname = computed(() => store.getIsNickname);
const isModal = ref(false);
const nickname = ref("");

const checkNickname = (e) => {
    if (e.target.value.length < 3 || e.target.value.length > 10) {
        store.setNicknameMessage("3자 이상 10자 이내의 한글, 영문만 가능해요");
        store.setIsNickname(false);
    } else {
        store.checkNickname(e.target.value);
    }
};

const handleNicknameClick = (e) => {
    isModal.value = true;
    store.changeMypageNickname(localStorage.getItem("userId"), nickname.value);
};

const handleChangeSuccess = () => {
    router.push({ name: "mypage" });
};

onMounted(() => {
    commonStore.headerTitle = "닉네임 수정";
    commonStore.headerType = "header2";
});
</script>

<style>
.nicknameview-main-container {
    display: flex;
    flex-direction: column;
    height: 100%;
    padding-top: 1rem;
    margin: 0 1rem 0 1rem;
}

.nicknameview-main-title {
    font-family: "Bold";
    font-size: 1.6rem;
    color: var(--black-color);
    margin: 0 0 2rem 0;
    user-select: none;
}

.nicknameview-input-label {
    font-family: "Medium";
    font-size: 0.8rem;
    color: var(--gray2-color);
    user-select: none;
}

.nicknameview-input-input {
    font-family: "Regular";
    font-size: 1.2rem;
    color: var(--gray2-color);
    width: 100%;
    height: 2.25rem;
    border: none;
    border-bottom: 0.1rem solid var(--gray-color);
    transition: border-bottom-color 0.1s;
}

.nicknameview-input-input:focus {
    outline: none;
    color: var(--main-color);
    border-bottom-color: var(--main-color);
}

.nicknameview-input-message {
    font-family: "Regular";
    font-size: 0.8rem;
    color: var(--gray2-color);
    margin-top: 0.5rem;
    user-select: none;
}

.nicknameeview-main-button {
    position: fixed;
    bottom: 4rem;
}
</style>
