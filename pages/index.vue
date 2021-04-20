<template>
  <div class="m-h-screen m-h-screen">
    <div>
      <p class="text-4xl mr-16 font-bold mt-16 text-center">#todo</p>
    </div>
    <div class="flex justify-center flex-wrap">
      <div class="smlaptop:w-4/12 flex mt-8 flex-row overflow-x-hidden justify-center border-b border-gray-200">
        <div @click="ActiveTab = 1" :class="{ 'border-b-2 border-indigo-600': ActiveTab === 1 }" class="mx-4 smlaptop:mx-8 px-4 py-1 cursor-pointer">
          <p class="text-lg">All</p>
        </div>
        <div @click="ActiveTab = 2" :class="{ 'border-b-2 border-indigo-600': ActiveTab === 2 }" class="mx-4 smlaptop:mx-8 px-4 py-1 cursor-pointer">
          <p class="text-lg">Active</p>
        </div>
        <div @click="ActiveTab = 3" :class="{ 'border-b-2 border-indigo-600': ActiveTab === 3 }" class="mx-4 smlaptop:mx-8 px-4 py-1 cursor-pointer">
          <p class="text-lg">Completed</p>
        </div>
      </div>
    </div>
    <div class="w-full flex justify-center">
      <div v-if="ActiveTab === 1 || ActiveTab === 2" class="smlaptop:w-4/12 flex flex-row">
        <input placeholder="add details" v-model="NewTask" type="text" class="w-10/12 focus:outline-none flex-grow h-12 mt-8 border-2 rounded-xl pl-4 border-gray-200">
        <button @click="AddNewTask()" class="ml-3 bttnn mt-8 rounded-xl px-8 py-2">Add</button>
      </div>
    </div>
    <div class="flex justify-center mt-4">
      <div class="w-full pl-12 smlaptop:pl-0 smlaptop:w-1/3" v-if="ActiveTab === 1">
        <div v-for="(task, i) in tasks" :key="i" @click="task.status = !task.status" class="smlaptop:w-8/12 mt-4 flex justify-start items-center cursor-pointer">
          <input type="checkbox" v-model="task.status" class="checked:bg-gray-200 smlaptop:w-6 h-6 cursor-pointer checked:border-red">
          <strike class="font-sans pl-4 text-base text-hardBlue" v-if="task.status">{{ task.name }}</strike>
          <p class="font-sans pl-4 text-base" v-else>{{ task.name }}</p>
        </div>
      </div>
      <div class="w-full pl-12 smlaptop:pl-0 smlaptop:w-1/3" v-if="ActiveTab === 2">
        <div v-for="(task, i) in pendingTasks" :key="i" class="smlaptop:w-8/12 mt-4 flex justify-start items-center cursor-pointer">
          <input disabled type="checkbox" v-model="task.status" class="checked:bg-gray-200 smlaptop:w-6 h-6 cursor-pointer checked:border-red">
          <strike class="font-sans pl-4 text-base text-hardBlue" v-if="task.status">{{ task.name }}</strike>
          <p class="font-sans pl-4 text-base" v-else>{{ task.name }}</p>
        </div>
      </div>
      <div class="w-full pl-12 smlaptop:pl-0 smlaptop:w-1/3" v-if="ActiveTab === 3">
        <div v-for="(task, i) in completedTasks" :key="i" class="mt-4 flex justify-between items-center cursor-pointer">
          <div class="smlaptop:w-4/5 flex items-center">
            <input disabled type="checkbox" v-model="task.status" class="checked:bg-gray-200 smlaptop:w-6 h-6 cursor-pointer checked:border-red">
            <strike class="font-sans pl-4 text-base text-hardBlue" v-if="task.status">{{ task.name }}</strike>
            <p class="font-sans pl-4 text-base" v-else>{{ task.name }}</p>
          </div>
          <img @click="deleteItem(task.name)" class="mr-4 smlaptop:mr-0" src="@/static/delete.png" alt="">
        </div>
        <div v-if="completedTasks.length === 0" class="flex justify-center mt-8">
          <p class="font-sans text-base">No task has been marked as completed</p>
        </div>
        <div v-if="completedTasks.length>0" class="flex justify-end mt-8">
          <button @click="deleteAllTasks()" class="mr-4 smlaptop:mr-0 flex rounded focus:outline-none flex-row px-8 py-2 btnred">
            <img src="@/static/delete.png" alt=""><p class="ml-3">delete all</p>
          </button>
        </div>
      </div>
    </div>
  </div>
</template>
<script>
export default {
  data () {
    return {
      ActiveTab: 1,
      tasks: [
        {
          name: 'First Task',
          status: false
        },
        {
          name: 'Second Task',
          status: false
        }
      ],
      NewTask: ''
    }
  },
  computed: {
    pendingTasks () {
      return this.tasks.filter(key => key.status === false)
    },
    completedTasks () {
      return this.tasks.filter(key => key.status === true)
    }
  },
  methods: {
    deleteAllTasks () { // delete all completed tasks
      this.completedTasks.map((key) => {
        const id = this.tasks.findIndex(item=> item.name === key.name)
        if (id !== -1) {
          this.tasks.splice(id, 1)
        }
      })
    },
    deleteItem (taskName) { // delete perticular task
      const id = this.tasks.findIndex(item=> item.name === taskName)
      this.tasks.splice(id, 1)
    },
    AddNewTask () {
      if (this.NewTask === '') {
        return
      }
      const newT = {
        name: this.NewTask,
        status: false
      }
      this.tasks.push(newT)
      this.NewTask = ''
    }
  }
}
</script>

<style>
.bttnn {
  background: #2F80ED;
  color: white;
}
.btnred {
  background: #EB5757;
  color: white;
}
</style>