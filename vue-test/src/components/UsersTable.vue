<template>
    <div class="table">
        <div class="table__header">
            <p class="table__name">Имя</p>
            <p class="table__phone">Телефон</p>
        </div>
        <div class="table__content">
            <UserRecord 
                v-for="(user, index) in usersData"
                :key="index"
                :user="user"
            />
        </div>
    </div>
</template>

<script>
import UserRecord from './UserRecord.vue';
import '../assets/style/variables.css';

export default {
    name: 'UsersTable',
    components: {
        UserRecord
    },
    props: {
        usersData: {
            type: Array,
            default: () => []
        }
    },
    data() {
        return {
            
        }
    }
}
</script>

<style scoped>
.table {
    width: 60%;
    min-width: 300px;
    margin: auto;
    border: solid var(--border-width) var(--primary-color);
    border-radius: var(--border-radius);
}

.table__header {
    display: flex;
    justify-content: flex-start;
    align-items: flex-start;
}

.table__name {
    border-right: solid var(--border-width) var(--primary-color);
}

p {
    flex: 1;
    display: flex;
    margin: 0;
    padding: 10px 0 5px 10px;
    font-weight: 600;
    color: rgba(0, 0, 0, 0.593);
}
</style>
