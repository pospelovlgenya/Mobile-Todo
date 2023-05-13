<template>
    <Page class="page">        
      <ActionBar class="ab">
        <Label text="Change Task" class="header"/>
      </ActionBar>

      <GridLayout rows="*, auto" columns="*">  
        <FlexboxLayout row="0" flexDirection="column">
          <TextView v-model="name" editable="true" />
          <TextView v-model="description" editable="true" />
        </FlexboxLayout>
      
        <FlexboxLayout row="1" flexDirection="column">
          <GridLayout rows="auto" columns="*, *, *">
            <Image col="0" src="~/assets/NotDone.png" height="50" width="33%" stretch="aspectFit" @tap="GoToHome()" />
            <Image col="1" src="~/assets/Recycle bin.png" height="50" width="33%" stretch="aspectFit" />
            <Image col="2" src="~/assets/Done.png" height="50" width="33%" stretch="aspectFit" />
          </GridLayout>
        </FlexboxLayout>
      </GridLayout>
    </Page>
</template>
    
<script>
import Home from './Home.vue';
export default {
  props: ['id', 'name', 'description'],
  data () {
    return {};
  },
  methods: {
    GoToHome() {
      this.$navigateTo(Home);
    },
  }
};
</script>
    
<style scoped lang="scss">

</style>
