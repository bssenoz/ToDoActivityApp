<template>
    <v-container style="width:500px">
      <v-row>
        <v-col>
          <v-text-field
      v-model="task.title"
      label="Başlık"
      variant="solo" outlined
    >
    </v-text-field>
        <v-text-field
      v-model="task.text" outlined
      label="İçerik"
      variant="solo"
    >
    </v-text-field>

<v-select
          label="Tarih"
          v-model="task.timed"
          :items="dateOptions"
          item-text="title"
          item-value="value"
          variant="solo" outlined
        ></v-select>

      <v-text-field v-if="task.timed === certain.value"
      v-model="task.startTime"
      label="Başlangıç Tarihi"
      type="date"
      variant="solo" outlined
      >
      </v-text-field>
<v-text-field v-if="task.timed === certain.value"
      v-model="task.endTime"
      label="Bitiş Tarihi"
      type="date"
      variant="solo" outlined
    >
    </v-text-field>

    <v-text-field
      v-model="task.location"
      label="Konum"
      variant="solo" outlined
    >
    </v-text-field>

    <v-text-field
      v-model="task.budget"
      label="Bütçe"
      variant="solo" outlined
    >
    </v-text-field>

        <v-btn @click="UpdateActivity">Güncelle</v-btn>
        </v-col>
      </v-row>
    </v-container>
  </template>
  
<script>
import { ref } from 'vue';
import axios from 'axios';
import Swal from 'sweetalert2';

export default {
  props: {
    task: Object,
  },
  setup(props) {

    const certain = {title: 'Belli',value: true};
    const uncertain = {title: 'Belirsiz',value: false};
    const dateOptions = [certain, uncertain];

    const token = localStorage.getItem("x-access-token");
 
 const UpdateActivity = async () => {
   const dialogs = document.querySelectorAll('.v-overlay__content');
  
   try {
        const response = await axios.put(`/api/Activity/UpdateActivity/${props.task.activityId}`,
        {
          title: props.task.title,
          text: props.task.text,
          startTime: props.task.startTime,
          endTime: props.task.endTime,
          budget: props.task.budget,
          location: props.task.location,
          timed: props.task.timed,
          done: props.task.done,
        }, {
          headers: {
            'Authorization': `Bearer ${token}`,   
            'Content-Type': 'application/json'    
          },
        });

        if (response.status === 200) {
          await Swal.fire({
                title: 'Planını güncelledin!',
                icon: 'success',
                confirmButtonText: 'Tamam',
                didOpen: () => {
                 const swalCont = document.querySelector('.swal2-container');

                  if (dialogs.length > 0) {
                    dialogs.forEach(dialog => {
                      dialog.style.zIndex = -2;
                    });
                  }
              swalCont.style.zIndex = 10000;
            },
          }).then(() => {
            dialogs[0].style.zIndex = 0;
            window.location.reload
          });
        }
      } catch (error) {
        await Swal.fire({
            title: 'Planını güncelleyemedik!',
            text: 'Üzgünüz, bir hata oluştu.',
            icon: 'error',
            confirmButtonText: 'Tamam',
            didOpen: () => {
              const swalCont = document.querySelector('.swal2-container');
              swalCont.style.zIndex = 10000;
            },
          }).then(() => {
            dialogs[0].style.zIndex = 0;
          })
        console.error(error);
      }
 }
   
    return { 
      UpdateActivity,
      certain,
      uncertain,
      dateOptions};
  },
};
</script>
  
  <style scoped>
.v-list-item--density-default {
    display: flex;
    justify-content: center;
    align-items: center;
    cursor: pointer;
}
.v-row {
  cursor: pointer;
}
</style>