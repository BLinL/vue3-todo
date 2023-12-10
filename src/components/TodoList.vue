<template>
    <div class="bg">
        <div class="main-container">
            <h1>Good!!! you have done {{ doneNum }}</h1>
            <div style="text-align: left; margin-bottom: 10px;">
                <input v-model="allCheck" type="checkbox" @click="handleAllDone(allCheck)" />
                all have done?

                <input v-model="toAddData" 
                placeholder="enter new item"
                @keyup.enter.native="addTodoItem" />
            </div>

            <div class="todo-container">
                <div class="todo-item-container" v-for="(todo, index) in todos.todoItems" key="index">
                    <input :id="randomId + '-' + index" :checked="todo.status === 1" @click="handleClick(index)" class="item"
                        name="todo" type="checkbox" />
                    <label :for="randomId + '-' + index">
                        <div class="todo-item"  :title="itemTitle(todo)"
                            @mouseover="itemTitle(todo)">
                            <h3 :class="todo.status === 1 ? 'overline' : 'normal'"
                             style="margin-left: 5px; display: inline-block; width: 100px; text-align: left;">{{
                                todo.content
                            }}</h3>
                            <button style="margin-left: 10px;" @click="handleRemove(index)">del</button>
                        </div>
                    </label>
                </div>
            </div>

        </div>
    </div>
</template>
<script setup lang="ts">
import moment from 'moment';
import { ref, computed, reactive } from 'vue';
interface TodoItem {
    content: string,
    status: -1 | 1 | 2,
    create?: Date
    update?: Date
}

const todoItems: TodoItem[] = [
    { content: 'haha', status: -1, create: new Date() },
    { content: 'haha1', status: -1, create: new Date() },
    { content: 'haha2', status: 1, create: new Date(), update: new Date() },
    { content: 'haha3', status: 2, create: new Date() },
]
let todos = reactive({
    todoItems
});

let allCheck = ref(false);

const handleClick = (idx1) => {
    const t = todos.todoItems[idx1];
    t.status = t.status == 1 ? t.status = -1 : t.status = 1;
    if (t.status === 1) {
        t.update = new Date();
    } else {
        t.update = undefined;
    }
}

const doneNum = computed(() => todos.todoItems.filter(a => a.status == 1).length);

const itemTitle = (item: TodoItem): string => {
    var t = moment(item.create).fromNow();
    if (item.update) {
        t += ',done at ' + moment(item.update).format('Y-MM-D HH:mm:ss');
    }

    return t;
}

const handleRemove = (idx: number): void => {
    todos.todoItems = todos.todoItems.filter((_, id) => id !== idx);
}

const handleAllDone = (checked: boolean) => {
    checked = !checked;
    const temp = todos.todoItems.map((item) => {
        const updateStatus = checked ? 1 : -1;
        item.status = updateStatus;
        return item;
    });

    todos.todoItems = temp;
}

const toAddData = ref('')
const addTodoItem = () => {
    todos.todoItems.push({content: toAddData.value, status: -1, create: new Date()})
}
const randomId = (Math.random() * 10).toFixed(5);
</script>
<style lang="css">
.todo-item-container {
    display: flex;
    align-items: center;
    width: 100%;
   
}

.todo-container {
    border-radius: 10px 10px 0 0;
    background-color: rgb(54, 77, 69, 0.7);
}

.main-container {
    z-index: 999;
}

.checkbox {
    margin-right: 10px;
}

.overline {
    text-decoration: line-through;
    color: rgba(207, 190, 190, 0.2);
    font-style: italic;
}

.normal {
    text-decoration:none;
}

.todo-item {
    color: yellow;
    cursor: pointer;
    transition: text-decoration 0.3s ease-out, color 0.3s ease-out;
}

.todo-item:hover {
    /* font-size: 20px; */
}

.bg {
    margin: 10px;
    background-image: url('src/assets/74143.jpg');
    background-size: cover;
    border-radius: 10px 10px 0 0;
    /* 模糊效果的强度，可以根据需要调整 */
}
</style>