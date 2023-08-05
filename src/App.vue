<template>
    <div class="w-full bg-blue-900 overflow-hidden min-h-screen font-[Poppins]">
        <div
            class="mx-4 space-y-2 md:mx-0 md:w-3/4 lg:w-1/2 grid grid-cols-1 rounded-2xl bg-white px-2 md:px-8 py-6
            md:py-12 my-6 md:my-12 md:mx-auto">

            <div class="col-span-1 space-y-3">
                <the-header
                    :brand="'texnomart'"
                    :title="'To do list'"
                    class="mb-6 md:mb-12"
                />

                <div class="w-full flex">
                    <app-input
                        :placeholder="'Add tasks...'"
                        v-model:task-name.trim="value"
                        @keyup.enter="addOrUpdate"
                        :class="currentToDoId !== null ? 'focus:border-green-400' : ''"
                        class="w-full px-4 md:px-6 md:py-3 border-2 border-r-0 rounded rounded-r-none outline-none
                               focus:border-blue-400 focus:placeholder-blue-500 duration-500 placeholder:text-gray-500
                               h-10 md:h-auto"
                    />

                    <app-button
                        v-if="currentToDoId === null"
                        @addToDo="addTask"
                        class="border-blue-500 bg-blue-500 hover:bg-blue-400 hover:border-blue-400"
                    >
                        <plus-icon class="w-4 h-4 md:w-6 md:h-6"/>
                    </app-button>

                    <app-button
                        v-else
                        @click="updateTask"
                        class="border-green-500 bg-green-500 hover:bg-green-400 hover:border-green-400"
                    >
                        <pencil-icon class="w-4 h-4 md:w-6 md:h-6"/>
                    </app-button>
                </div>
            </div>

            <div class="col-span-1 space-y-2">
                <div
                    v-for="(task, index) in toDoList" :key="task.id"
                    class="flex justify-between items-center px-4 md:px-6 py-2 bg-blue-500 rounded shadow
                    space-x-2 bg-gray-200"
                >
                    <div class="flex space-x-2 items-center">
                        <app-input
                            :type="'checkbox'"
                            :checked="task.checked"
                            v-model="task.checked"
                            @click="checkOrRemoveCheck(index)"
                            class="w-4 h-4 md:w-5 md:h-5"
                        />
                        <span
                            class="text-lg md:text-xl"
                            :class="task.checked ? 'text-blue-500' : ''"
                        >{{ index + 1 + '.' }}</span>
                        <span
                            :class="task.checked ? 'text-blue-500' : ''"
                            class="text-lg md:text-xl">{{ task.title }}
                        </span>
                    </div>
                    <div class="flex space-x-2 items-center">

                        <div v-if="task.checked" class="flex space-x-1 text-blue-500 mr-10">
                            <hand-thumb-up-icon class="w-4 h-4 md:w-6 md:h-6"/>
                            <span>Done</span>
                        </div>
                        <div v-else class="flex space-x-1 text-red-500 mr-10">
                            <hand-thumb-down-icon class="w-4 h-4 md:w-6 md:h-6"/>
                            <span>Fail</span>
                        </div>
                        <span
                            class="text-end cursor-pointer text-green-500 hover:text-green-400 hover:scale-125 duration-500"
                            @click="editTask(task)"
                        >
                            <PencilSquareIcon class="w-4 h-4 md:w-6 md:h-6"/>
                        </span>
                        <span
                            class="text-end cursor-pointer text-red-500 hover:text-red-400 hover:scale-125 duration-500"
                            @click="removeTask(index)"
                        >
                            <XMarkIcon class="w-4 h-4 md:w-6 md:h-6"/>
                        </span>
                    </div>
                </div>
            </div>

            <div v-if="!toDoList.length" class="text-center mt-12">
                <h1 class="font-semibold text-3xl text-fuchsia-700">There is no any tasks</h1>
            </div>

            <div class=" border-t-2 border-blue-500"></div>

            <div class="px-4 md:px-6 space-y-2 font-bold text-lg md:text-xl">
                <h2 v-if="completedTasks" class="text-blue-500">
                    Completed tasks:
                    <span class="text-gray-700">{{ completedTasks }}</span>
                </h2>
                <h2 v-if="notCompletedTasks" class="text-red-500">
                    Not completed tasks:
                    <span class="text-gray-700">{{ notCompletedTasks }}</span>
                </h2>
            </div>

        </div>
    </div>
</template>

<script>
import {
    PencilSquareIcon,
    XMarkIcon,
    PlusIcon,
    PencilIcon,
    HandThumbUpIcon,
    HandThumbDownIcon
} from '@heroicons/vue/24/outline'
import TheHeader from "@/components/TheHeader.vue";
import AppInput from "@/components/AppInput.vue";
import AppButton from "@/components/AppButton.vue";

export default {
    components: {
        AppButton,
        AppInput,
        TheHeader,
        PencilSquareIcon,
        XMarkIcon,
        PlusIcon,
        PencilIcon,
        HandThumbUpIcon,
        HandThumbDownIcon
    },
    data() {
        return {
            idCreater: 0,
            value: '',
            currentToDoId: null,
            checked: false,
            toDoList: []
        }
    },
    computed: {
        completedTasks() {
            return this.toDoList.filter(el => el.checked === true).length
        },
        notCompletedTasks() {
            return this.toDoList.filter(el => el.checked === false).length
        }
    },
    methods: {
        addTask() {
            if (this.value.length) {
                this.toDoList.push({
                    id: this.idCreater++,
                    title: this.value[0].toUpperCase() + this.value.slice(1),
                    checked: this.checked
                })
                this.value = ''

                this.saveData()
            }
        },
        saveData() {
            let data = JSON.stringify(this.toDoList)
            localStorage.setItem('todoList', data)
            this.render()
        },
        render() {
            if (JSON.parse(localStorage.getItem('todoList'))) {
                this.toDoList = JSON.parse(localStorage.getItem('todoList'))
            }
        },
        editTask(item) {
            this.value = item.title
            this.currentToDoId = item.id
        },
        updateTask() {
            if (this.value.length) {
                const index = this.toDoList.findIndex(el => el.id === this.currentToDoId)
                this.toDoList[index].title = this.value
                this.currentToDoId = this.value = null
            }

            this.saveData()
        },
        checkOrRemoveCheck(data) {
            this.toDoList[data].checked = !this.toDoList[data].checked;
            this.saveData()
        },
        addOrUpdate() {
            if (this.currentToDoId === null) {
                this.addTask()
            } else {
                this.updateTask()
            }
        },
        removeTask(index) {
            this.toDoList.splice(index, 1)
            this.value = ''
        },
    },
    mounted() {
        this.idCreater = this.toDoList.length + 1
        this.render()
    }
}
</script>

<style scoped>
</style>
