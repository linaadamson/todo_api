<template>
    <div class="todo" :class="{ cross: todo.completed === true }">
        <p>{{ todo.title }}</p>

        <div class="btn-container">
            <button class="complete" @click="updateTodoStatus">
                <img src="../../assets/icons/completed.svg" alt="complete" />
            </button>

            <button class="delete" @click="deleteTodo">
                <img src="../../assets/icons/delete.svg" alt="delete" />
            </button>
        </div>
    </div>
</template>

<script>
export default {
    name: 'TodoCard',
    props: {
        todo: {
            type: Object,
            required: true,
        }
    },
    data() {
        return {

        }
    },
    methods: {
        deleteTodo: function () {
            this.$emit("delete", this.todo)

        },
        updateTodoStatus: function () {
            this.$emit("updateStatus", this.todo)

        }
    }

}
</script>

<style lang="css" scoped>
@import './index.css';
</style>
