<template>
    <Page class="page">
        <ActionBar>
            <Label text="Todo"/>
        </ActionBar>
        
        <GridLayout rows="auto, *" columns="*">
          
          <GridLayout row="0" rows="auto" columns="*, *, *, *">
            <Switch col="0" checked="false" v-model="first"/>
            <Label col="1" text="In progress" textWrap="true"/>
            <Switch col="2" checked="false" v-model="second"/>
            <Label col="3" text="Complited" textWrap="true"/>
          </GridLayout>
          
          <ScrollView row="1" v-if="show.length">
            <FlexboxLayout flexDirection="column">
              <FlexboxLayout v-for="task in show" :key="task.id" flexDirection="column">
                <FlexboxLayout flexDirection="center">
                  <GridLayout rows="*, 2*" columns="*">
                    <label row="0" :text="task.name"/>
                    <label row="1" :text="task.description"/>
                  </GridLayout>
                  <Image src="~/assets/Done.png" height="70" width="33%" stretch="aspectFit" v-if="task.complete"/>
                  <Image src="~/assets/NotDone.png" height="70" width="33%" stretch="aspectFit" v-else/>
                </FlexboxLayout>
              </FlexboxLayout>
            </FlexboxLayout>
          </ScrollView>

          <label row="1" text="Задач не обнаружено" v-else/>
            
      </GridLayout>
        
    </Page>
</template>

<script>

import * as ApplicationSettings from '@nativescript/core/application-settings';
export default {
    data() {
        return {
            first: false,
            second: false,
            tasks: [
              { id: 0, name: "1", description: "d1", complete: true },
                { id: 1, name: "2", description: "d2", complete: true },
                { id: 2, name: "3", description: "d3", complete: false },
                { id: 3, name: "4", description: "d4", complete: false },
                { id: 4, name: "5", description: "d5", complete: false },
                { id: 5, name: "6", description: "d6", complete: true },
                { id: 6, name: "7", description: "d7", complete: true },
                { id: 7, name: "8", description: "d8", complete: false },
                { id: 8, name: "9", description: "d9", complete: false },
                { id: 9, name: "10", description: "d10", complete: true },
                { id: 10, name: "11", description: "d11", complete: true },
                { id: 11, name: "12", description: "d12", complete: true },
                { id: 12, name: "13", description: "d13", complete: false },
                { id: 13, name: "14", description: "d14", complete: false },
              ],
              show: [],
            };
    },
    beforeMount() {
      if (ApplicationSettings.getString("tasks")) {
        this.tasks = Object.values(JSON.parse(ApplicationSettings.getString("tasks")));
      }
      this.GetShow();
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
        if (this.first && this.second) {
          this.show = this.tasks;
          return
        }
        if (this.first) {
          this.show = this.tasks.filter(item => item.complete == false);
          return
        }
        if (this.second) {
          this.show = this.tasks.filter(item => item.complete == true);
          return
        }
        this.show = this.tasks;
      },
      save() {
        ApplicationSettings.setString("tasks", JSON.stringify(Object.assign({}, this.tasks)));
      },
    },
};
</script>

<style scoped lang="scss">
    @import '@nativescript/theme/scss/variables/blue';

</style>
