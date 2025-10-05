<template>
  <BaseCard>
    <BaseButton
      @click="setSelectedTab('stored-resources')"
      :mode="storedResButtonMode"
      >Stored Resources</BaseButton
    >
    <BaseButton @click="setSelectedTab('add-resource')" :mode="addResButtonMode"
      >Add Resource</BaseButton
    >
  </BaseCard>
  <keep-alive><component :is="selectedTab"></component></keep-alive>
</template>
<script>
import StoredResources from './StoredResources.vue';
import AddResource from './AddResource.vue';

export default {
  components: { StoredResources, AddResource },
  data() {
    return {
      selectedTab: 'stored-resources',
      storedResources: [
        {
          id: 'official-guide',
          title: 'Official Guide',
          description: 'The official Vue.js documentation.',
          link: 'https://vue.js.org',
        },
        {
          id: 'google',
          title: 'Google',
          description: 'Learn to google...',
          link: 'https://google.org ',
        },
      ],
    };
  },
  provide() {
    return {
      resources: this.storedResources,
      addResource: this.addResource,
      deleteResource: this.removeResource,
    };
  },
  computed: {
    storedResButtonMode() {
      return this.selectedTab === 'stored-resources' ? '' : 'flat';
    },
    addResButtonMode() {
      return this.selectedTab === 'add-resource' ? '' : 'flat';
    },
  },
  methods: {
    setSelectedTab(tab) {
      this.selectedTab = tab;
    },
    addResource(title, description, url) {
      const newResource = {
        id: new Date().toISOString(),
        title,
        description,
        link: url,
      };

      this.storedResources.unshift(newResource);
      this.selectedTab = 'stored-resources';
    },

    removeResource(resId) {
      const resIdex = this.storedResources.findIndex((res) => res.id === resId);
      this.storedResources.splice(resIdex, 1);
    },
  },
};
</script>
