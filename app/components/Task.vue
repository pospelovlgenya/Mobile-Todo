<template>
  <Page class="page">
    <ActionBar class="ab">
      <Label text="Change Task" class="header" />
    </ActionBar>

    <GridLayout rows="*, auto" columns="*">

      <FlexboxLayout row="0" flexDirection="column">
        <Label text="Name" />
        <TextField v-model="name" editable="true" maxLength="35" class="namechange" />
        <Label text="Description" />
        <TextField v-model="description" editable="true" maxLength="35" class="descchange" />
        <Image src="~/assets/pumpkin.png" stretch="aspectFit" />
      </FlexboxLayout>

      <FlexboxLayout row="1" flexDirection="column">
        <GridLayout rows="auto" columns="*, *, *">
          <Image col="0" src="~/assets/NotDone.png" height="50" width="33%" stretch="aspectFit" @tap="GoToHome()" class="button" />
          <Image col="1" src="~/assets/Recycle bin.png" height="50" width="33%" stretch="aspectFit" @tap="DeleteTask()" class="button" />
          <Image col="2" src="~/assets/Done.png" height="50" width="33%" stretch="aspectFit" @tap="ChangeTask()" class="button" />
        </GridLayout>
      </FlexboxLayout>

    </GridLayout>
  </Page>
</template>
    
<script>
import Home from './Home.vue';
export default {
  props: ["id", "name", "description"],
  data() {
    return {};
  },
  methods: {
    GoToHome() {
      this.$navigateTo(Home);
    },
    DeleteTask() {
      this.$navigateTo(Home, { props: { met: "del", taskdata: this.id } });
    },
    ChangeTask() {
      this.$navigateTo(Home, { props: { met: "che", taskdata: [this.id, this.name, this.description] } });
    }
  },
};
</script>
    
<style scoped lang="scss"></style>
