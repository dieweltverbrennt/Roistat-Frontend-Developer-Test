<template>
    <div class="form__background">
        <div class="form__wrapper">
            <div class="form__header">
                <h3>Добавление пользователя</h3>
                <button class="form__close"
                    @click="$emit('close-form')"
                >
                    &#10006;
                </button>
            </div>
            <form @submit.prevent="addUser">
                <label>
                    Имя
                    <input 
                        type="text" 
                        v-model="name" 
                        required
                    >
                </label>

                <label>
                    Телефон
                    <input 
                        type="text" 
                        v-mask="'+7 ### ### ## ##'" 
                        v-model="phone" 
                        required
                    >
                </label>

                <label>
                    Начальник
                    <select v-model="parent">
                        <option 
                            v-for="(option, index) in parentsList" 
                            :key="index" 
                            :value="option.id"
                        >
                            {{ option.name }} - {{ option.phone }}
                        </option>
                    </select>
                </label>

                <input type="submit" value="Сохранить">
            </form>
        </div>
    </div>
</template>

<script>
import { TheMask } from 'vue-the-mask';
import '../assets/style/variables.css';

export default {
    name: 'AddUserForm',
    components: {
        TheMask
    },
    props: {
        usersData: {
            type: Array,
            default: () => []
        },
        parentsList: {
            type: Array,
            default: () => []
        }
    },
    data() {
        return {
            name: '',
            phone: '',
            parent: null
        }
    },
    methods: {
        addUser() {
            this.$emit('add-user', { name: this.name, phone: this.phone }, this.parent);
            this.name = '';
            this.phone = '';
            this.parent = null;
        }
    }
}
</script>

<style scoped>
.form__background {
    position: absolute;
    top: 0;
    right: 0;
    bottom: 0;
    left: 0;
    background-color: rgba(0, 0, 0, 0.249);
    display: flex;
    justify-content: center;
    align-items: center;
}

.form__wrapper {
    max-width: 650px;
    width: 60%;
    background-color: #fff;
    padding: 10px 20px 40px;
    border-radius: var(--border-radius);
}

.form__header {
    display: flex;
    justify-content: space-between;
    margin-bottom: 40px;
}

.form__close {
    all: unset;
    font-size: 20px;
    cursor: pointer;
}

form {
    display: flex;
    gap: 20px;
    flex-direction: column;
    padding: 0 80px;
}

form > :last-child {
    margin-top: 20px;
    background-color: var(--primary-color);
    border: none;
    border-radius: var(--border-radius);
    padding: 10px 20px;
    color: #fff;
    text-transform: uppercase;
    font-weight: 600;
    cursor: pointer;
}

label {
    display: flex;
    justify-content: space-between;
}

input, select {
    width: 70%;
    align-self: center;
    border: 2px solid rgb(182, 182, 182);
    border-radius: 2px;
    outline-color: var(--primary-color);
    box-sizing: border-box;
}
</style>