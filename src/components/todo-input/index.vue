<template>
    <div>
        <div class="input-container">
            <input v-model="newTodo" type="text" placeholder="Create a new todo..." class="todo-input"
                @focus="error = ''" />
            <button class="add-btn" type="submit" @click="createTodo">
                <img src="../../assets/icons/addIcon.svg" alt="Apple store logo" />
            </button>
            <span class="error" v-if="error">{{ error }}</span>
        </div>

    </div>
</template>

<script>
export default {
    name: 'TodoInput',

    data() {
        return {
            newTodo: '',
            error: '',
        }
    },

    methods: {
        createTodo: function () {
            if (!this.newTodo.trim().length) {
                this.error = 'Please include a todo'
                console.log(this.error)
                return
            }
            this.$emit("add", this.newTodo)
            this.newTodo = '';
            this.error = ''
        }
    }

}
</script>

<style lang="css" scoped>
@import './index.css';
</style>
