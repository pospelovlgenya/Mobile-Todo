<template>
  <Page class="page">
      <ActionBar class="ab">
        <Label text="Todo" class="header"/>
      </ActionBar>
      
      <GridLayout rows="auto, *, auto" columns="*">
        
        <GridLayout row="0" rows="auto" columns="*, *">
          <GridLayout col="0" rows="auto" columns="auto, *" class="button">
            <Switch col="0" checked="false" v-model="first"/>
            <Label col="1" text="In progress" textWrap="true"/>
          </GridLayout>
          <GridLayout col="1" rows="auto" columns="auto, *" class="button">
            <Switch col="0" checked="false" v-model="second"/>
            <Label col="1" text="Complited" textWrap="true"/>
          </GridLayout>
        </GridLayout>
        
        <ScrollView row="1" v-if="show.length">
          <FlexboxLayout flexDirection="column">
            <FlexboxLayout v-for="task in show" :key="task.id" flexDirection="column" class="fulltask">
              <FlexboxLayout flexDirection="center">
                <GridLayout rows="*, *" columns="*" @tap="GoToTask(task.id)">
                  <label row="0" :text="task.name" class="taskname"/>
                  <label row="1" :text="task.description" class="taskdesc"/>
                </GridLayout>
                <Image src="~/assets/Done.png" height="70" width="33%" stretch="aspectFit" @tap="changeComplete(task.id)" v-if="task.complete" class="taskimg"/>
                <Image src="~/assets/NotDone.png" height="70" width="33%" stretch="aspectFit" @tap="changeComplete(task.id)" v-else class="taskimg"/>
              </FlexboxLayout>
            </FlexboxLayout>
          </FlexboxLayout>
        </ScrollView>

        <label row="1" text="Задач не обнаружено" v-else/>
          
        <FlexboxLayout row="2" flexDirection="column" @tap="newTask()" >
          <Image src="~/assets/Add.png" height="50" width="33%" stretch="aspectFit" class="button" />
        </FlexboxLayout>
    </GridLayout>
      
</Page>
</template>

<script>
import * as ApplicationSettings from '@nativescript/core/application-settings';
import Task from './Task.vue';
export default {
    props: ['met', 'taskdata'],
    data() {
        return {
            first: false,
            second: false,
            tasks: [],
            show: [],
            };
    },
    beforeMount() {
      if (ApplicationSettings.getString("tasks")) {
        this.tasks = Object.values(JSON.parse(ApplicationSettings.getString("tasks")));
      }
      if (this.met == 'del') {
        this.tasks.splice(this.taskdata, 1);
      }
      if (this.met == 'che') {
        this.tasks[this.taskdata[0]].name = this.taskdata[1];
        this.tasks[this.taskdata[0]].description = this.taskdata[2];
      }
      this.sortId();
      this.GetShow();
      this.save();
    },
    watch: {
      first: function() {
        this.GetShow();
      },
      second: function() {
        this.GetShow();
      },
    },
    methods: {
      GetShow() {
        if (this.first && !this.second) {
          this.show = this.tasks.filter(item => item.complete == false);
          return
        }
        if (this.second && !this.first) {
          this.show = this.tasks.filter(item => item.complete == true);
          return
        }
        this.show = this.tasks;
      },
      sortId() {
        for (var i = 0; i < this.tasks.length; i++) {
          this.tasks[i].id = Number(i);
        }
      },
      save() {
        ApplicationSettings.setString("tasks", JSON.stringify(Object.assign({}, this.tasks)));
      },
      changeComplete(id) {
        this.tasks[id].complete = !(this.tasks[id].complete);
        this.save();
      },
      GoToTask: function(id) {
        this.$navigateTo(Task, { props: {id: id, name: this.tasks[id].name, description: this.tasks[id].description}});
      },
      newTask() {
        var nextId = this.tasks.length;
        this.tasks.push({id: Number(nextId), name: 'New Task', description: 'New Task', complete: false});
        this.save();
        this.GoToTask(nextId);
      },
    },
};
</script>

<style scoped lang="scss">

</style>
