<template>
    <div class="container">
        <p1 >Add to do list</p1>
        <form id="new-todo-form" @submit.prevent="SaveData()">
            <input id="content" type="text" v-model="list.val"> <br />
            <div class="options">
                <label>
                    <input id="category1" type="radio" v-model="list.type" value="study" /><span>Study</span> 

                </label>
                <label>
                    <input id="category2" type="radio" v-model="list.type" value="private" /> <span >Private</span> 

                </label>
            </div><br />
            <button>Add</button>
        </form>

        <ul>
            <li v-for="(list_,index) in Lists" :key="list_.id">
                <input type="checkbox" @click="checkSataus(list_)"><span
                    :class="`${list_.done ?'done':'notdone'} ${list_.type == 'study' ?'study':'private'}`">
                    {{list_.val}}</span> <button @click="deleteData(index)">Delete</button>
            </li>
        </ul>
    </div>
</template>

<script>
export default {
    data() {
        return {
            list: {
                val: '',
                type: '',
                done: false,
            },
            Lists: []
        }
    },
    mounted() {

        const data = localStorage.getItem('todoList');
        this.Lists = JSON.parse(data);
        if (this.Lists === null) {
            this.Lists.push({
                val: 'Hello',
                type: 'Private',
                done: false,
            });
            localStorage.setItem('todoList', JSON.stringify(this.Lists));

        }
        console.log(this.Lists);


    },
    methods: {
        SaveData() {
            if (this.list.val === '') {
                alert('input Data');
                return;
            } if (this.list.type === '') {
                alert('select type');
                return;
            }

            this.Lists.push(this.list);
            localStorage.setItem('todoList', JSON.stringify(this.Lists));
            this.list = {
                val: '',
                type: '',
                done: false,
            };

        },
        deleteData(index) {
            console.log(index);
            this.Lists.splice(index, 1);
            localStorage.setItem('todoList', JSON.stringify(this.Lists));

        },
        checkSataus(list_) {
            return list_.done = !list_.done;

        }

    },
    watch: {

    }

}
</script>

<style scoped>
.done {
    text-decoration: line-through;
}

.notdone {
    text-decoration: none;
}

.study {
    color: blue;
}

.private {
    color: chartreuse;
}

</style>