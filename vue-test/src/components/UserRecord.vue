<template>
    <div class="table__record">
        <div class="table__name">
            <span 
                class="table__margin" 
                v-for="(item, index) in level" 
                :key="index"
            ></span>
            <button 
                v-if="user.children.length" 
                @click="toggleExpand"
            >
                <span class="table__expand" v-if="isExpanded">-</span>
                <span class="table__expand" v-else>+</span>
            </button>
            <span class="table__margin" v-else></span>
            <p>{{ user.name }}</p>
        </div>
        <p class="table__phone">{{ user.phone }}</p>
        <div class="table__child" v-if="user.children.length && isExpanded">
            <UserRecord 
                v-for="(child, index) in user.children"
                :key="index"
                :user="child"
                :level="level + 1"
            />
        </div>
    </div>
</template>

<script>
import '../assets/style/variables.css';

export default {
    name: 'UserRecord',
    props: {
        user: {
            type: Object,
            default: null
        },
        // определяет уровень вложенности
        level: {
            type: Number,
            default: 0
        }
    },
    data() {
        return {
            isExpanded: false
        }
    },
    methods: {
        toggleExpand() {
            this.isExpanded = !this.isExpanded;
        }
    }
}
</script>

<style scoped>

.table__record {
    display: flex;
    flex-wrap: wrap;
    justify-content: flex-start;
    align-items: flex-start;
    border-top: solid var(--border-width) var(--primary-color);
}

button {
    all: unset;
    width: 10px;
    display: flex;
}

.table__expand {
    font-size: 20px;
    font-weight: 600;
    color: var(--primary-color);
}

.table__margin {
    width: 10px;
}

p {
    margin: 0;
}

.table__phone, .table__name {
    flex: 1;
    display: flex;
    padding: 10px 0 5px 10px;
}

.table__name {
    border-right: solid var(--border-width) var(--primary-color);
    display: flex;
    align-items: center;
    gap: 5px;
}

.table__child {
    width: 100%;
    background-color: rgba(112, 154, 251, 0.09);
}

.table__child .table__record {
    border-top: none;
}
</style>