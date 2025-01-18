<template>
  <v-layout>
    <v-app-bar color="dark" prominent>
      <v-app-bar-nav-icon variant="text" @click.stop="drawer = !drawer"></v-app-bar-nav-icon>
      <v-toolbar-title>My files</v-toolbar-title>
      <v-spacer></v-spacer>
      <template v-if="$vuetify.display.mdAndUp">
        <v-btn icon="mdi-magnify" variant="text"></v-btn>
        <v-btn icon="mdi-filter" variant="text"></v-btn>
      </template>
      <v-btn icon="mdi-dots-vertical" variant="text"></v-btn>
    </v-app-bar>

    <v-navigation-drawer v-model="drawer" :location="$vuetify.display.mobile ? 'bottom' : undefined">
      <v-treeview 
        :items="items" 
        :opened="initiallyOpen" 
        item-value="id" 
        activatable
        density="compact"
        @update:activated="handleTreeItemActive"
      >
        <template v-slot:prepend="{ item, isOpen }">
          <v-icon v-if="!item.file">
            {{ isOpen ? 'mdi-folder-open' : 'mdi-folder' }}
          </v-icon>
          <v-icon v-else>
            {{ item.file }}
          </v-icon>
        </template>
      </v-treeview>
    </v-navigation-drawer>

    <!-- Right Drawer (File Details) -->
    <v-navigation-drawer
      v-model="drawerRight"
      location="right"
      :location="$vuetify.display.mobile ? 'bottom' : undefined"
      width="300"
    >
      <v-card>
        <v-card-title>File Details</v-card-title>
        <v-divider></v-divider>
        <v-card-text>
          <!-- Add content for file details here -->
          <v-card-subtitle v-if="activeFile">Type</v-card-subtitle>
          <v-card-subtitle v-if="activeFile">{{ activeFile.type === 'file' ? `file/${getFileExtension(activeFile.title)}` : 'folder' }}</v-card-subtitle>
        </v-card-text>
        <v-card-text>
          <v-card-subtitle v-if="activeFile">Created At</v-card-subtitle>
          <v-card-subtitle v-if="activeFile">{{ activeFile.created_at }}</v-card-subtitle>
        </v-card-text>
        <v-card-text>
          <v-card-subtitle v-if="activeFile">Updated At</v-card-subtitle>
          <v-card-subtitle v-if="activeFile">{{ activeFile.updated_at }}</v-card-subtitle>
        </v-card-text>
      </v-card>
    </v-navigation-drawer>

    <v-main height="100vh" width="100vw">
        <v-card
          class="fill-height"
          max-width="100vw"
          max-height="100vh"
        >
        <v-list>
          <v-list-item
            v-for="(item, index) in listItems"
            :key="index"
            @click="handleRowDoubleClick(item)"
          >
            <v-row align="center" no-gutters>
              <v-col class="d-flex pr-2" cols="auto">
                <v-icon>{{ item.type === 'file' ? getFileIcon(item.title) : 'mdi-folder' }}</v-icon>
              </v-col>
              <v-col>
                <v-list-item-title>{{ item.title }}</v-list-item-title>
              </v-col>
              <v-col cols="auto pa-2">
                <v-list-item-subtitle>{{ item.type === 'file' ? `file/${getFileExtension(item.title)}` : 'folder' }}</v-list-item-subtitle>
              </v-col>
              <v-col cols="auto pa-2">
                <v-list-item-subtitle>{{ item.updated_at }}</v-list-item-subtitle>
              </v-col>
            </v-row>
          </v-list-item>
        </v-list>
        </v-card>
    </v-main>
  </v-layout>
</template>

<script setup>
import { ref } from 'vue'
import axios from 'axios'

const drawer = ref(false)
const drawerRight = ref(false)  // Right Drawer for file details
const activeFile = ref(null)  // Store the active file's data

const initiallyOpen = ref(['public'])
const files = ref({
  html: 'mdi-language-html5',
  js: 'mdi-nodejs',
  json: 'mdi-code-json',
  md: 'mdi-language-markdown',
  pdf: 'mdi-file-pdf-box',
  png: 'mdi-file-image',
  txt: 'mdi-file-document-outline',
  xls: 'mdi-file-excel',
})

const listItems = ref([])
const headers = ref([
  { title: 'Plant', align: 'start', sortable: false, key: 'name' },
  { title: 'Light', align: 'end', key: 'light' },
  { title: 'Height', align: 'end', key: 'height' },
])

const items = ref([])

const defaultFileIcon = 'mdi-file'

function getFileExtension(fileName) {
  return fileName.split('.').pop().toLowerCase()
}

function getFileIcon(fileName) {
  return files.value[getFileExtension(fileName)] || defaultFileIcon
}

function formatItems(data) {
  return data.map(item => {
    const isFile = item.type === 'file';
    const formattedItem = {
      id: item.id,
      title: item.title,
      type: item.type,
      file: isFile ? getFileIcon(item.title) : null,
    };
    
    if (!isFile) {
      formattedItem.children = item.children && item.children.length > 0 
        ? formatItems(item.children) 
        : [];
    }

    return formattedItem;
  });
}

onBeforeMount(async () => {
  try {
    const response = await axios.get('http://localhost:3000/api/v1/items?fullHierarchy=true')
    if (response.data.status === 'success') {
      items.value.push(...formatItems(response.data.data))
    } else {
      console.error('API returned an error:', response.data)
    }
  } catch (error) {
    console.error('Failed to fetch items:', error)
  }
})

async function handleTreeItemActive(activeItems) {
  listItems.value = [];
  try {
    const response = await axios.get(`http://localhost:3000/api/v1/items?parentId=${activeItems[0]}`)
    if (response.data.status ==='success') {
      console.log("response: ", response.data.data)
      listItems.value.push(...response.data.data)
    } else {
      console.error('API returned an error:', response.data)
    }
  } catch (error) {
    
  }
}

function handleRowDoubleClick(item) {
  if (item.type === 'file') {
    activeFile.value = item;  // Set the active file
    drawerRight.value = true;  // Open the right drawer
  } else if (item.type === 'folder') {
    handleTreeItemActive([item.id])
  }
}


</script>