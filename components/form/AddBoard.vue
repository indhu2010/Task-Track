<template>
  <transition name="fade">
    <div v-if="boardFormState" class="popup-modal">
      <div
        class="w-fit h-fit flex flex-col p-8 bg-charcoal rounded-xl gap-10 relative m-10"
      >
        <button
          class="absolute right-0 translate-x-4 -translate-y-5 top-0 rounded-full bg-mauve p-3"
          @click="() => (boardFormState = false)"
        >
          <XMarkIcon class="w-5 h-5" />
        </button>

        <h2>Create a New Board</h2>

        <div class="flex flex-col gap-5">
          <label for="task_name">Board Name</label>
          <input
            autofocus
            v-model.trim="boardName"
            type="text"
            name="task_name"
            placeholder="e.g Learn Nuxt.js"
          />
        </div>


        <div class="flex items-center gap-2">
          <ColorPicker :color.sync="selectedColor"/>
          <BaseButton label="Create Board" @action="useCreateNewBoard" class="bg-savoy"/>
          <!-- Delete latest created board button -->
          <button class="bg-red-500 text-white rounded-md py-2 px-4" @click="deleteLatestCreatedBoard">Delete Board</button>
        </div>
      </div>
    </div>
  </transition>

  <!-- Render existing boards -->
  <!-- <div v-if="boards.length > 0">
    <h2>Existing Boards:</h2>
    <ul>
      <li v-for="board in boards" :key="board.id">
        {{ board.name }}
      </li>
    </ul>
  </div> -->
</template>

<script setup lang="ts">
import { ref } from "vue";
import { useKanbanStore } from "~~/stores";
import { XMarkIcon } from "@heroicons/vue/24/outline";

//Props and emits
const boardFormState = isAddBoardFormOpen();

//Refs
const boardName = ref<string>("");
const store = useKanbanStore();
const { createNewBoard, deleteBoard, boards } = store;

//Methods
const resetValues = (): void => {
  boardName.value = "";
};

const useCreateNewBoard = () => {
  if (useValidator(boardName.value)) {
    createNewBoard(boardName.value);
    resetValues();
    boardFormState.value = false;
  }
};

const deleteLatestCreatedBoard = (): void => {
  if (boards.length > 0) {
    const latestBoardId = boards[boards.length - 1].id;
    deleteBoard(latestBoardId);
  }
};
</script>
