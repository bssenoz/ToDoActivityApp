<template>
    <SideBarAdmin/>
    <v-container>
      <div class="text-h5 mb-4">Kayıtlı Kullanıcılar</div>
      <UserTable />
    </v-container>
 </template>

<script>
    import SideBarAdmin from "@/components/SideBarAdmin.vue";
    import UserTable from "@/components/admin/UserTable.vue";

    
    export default {
      components: {
        SideBarAdmin,
        UserTable
      },
      setup() {
    
        return {
        };
      },
    }
 </script>
    