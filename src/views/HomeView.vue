
<template>
  <v-form v-model="valid">
    <v-container>
      <v-card>
        <v-card-title>Funcionario que recepciona</v-card-title>
        <v-card-text>
          <v-row>
            <v-col cols="12" sm="4">
              <v-text-field v-model="firstNameOfficial" label="Nombres" :rules="[v => !!v || 'El nombre es requerido']"></v-text-field>
            </v-col>
            <v-col cols="12" sm="4">
              <v-text-field v-model="lastNameOfficial" label="Apellidos" :rules="[v => !!v || 'El apellido es requerido']"></v-text-field>
            </v-col>
            <v-col cols="12" sm="4">
              <v-text-field v-model="officialPosition" label="Cargo" :rules="[v => !!v || 'El cargo es requerido']"></v-text-field>
            </v-col>
          </v-row>
        </v-card-text>
      </v-card>
      <v-card>
        <v-card-title>Proceso vinculado</v-card-title>
        <v-container grid>
          <v-row>
            <v-col cols="12" sm="6">
              <v-checkbox v-model="technicalCheck" label="Tecnica"></v-checkbox>
            </v-col>
            <v-col cols="12" sm="6">
              <v-checkbox v-model="adminCheck" label="Administrativo"></v-checkbox>
            </v-col>
          </v-row>
        </v-container>
      </v-card>
      <v-card>
  <v-card-title>Funcionario responsable designado por Gerencia para dar tratamiento y respuesta.</v-card-title>
  <v-card-text>
    <v-row>
      <v-col cols="12" sm="6">
        <v-text-field v-model="firstNameManagement" label="Nombres" :rules="[v => !!v || 'El nombre es requerido']"></v-text-field>
      </v-col>
      <v-col cols="12" sm="6">
        <v-text-field v-model="lastNameManagement" label="Apellidos" :rules="[v => !!v || 'El apellido es requerido']"></v-text-field>
      </v-col>
      <v-col cols="12" sm="6">
        <v-menu ref="menu1" v-model="menu1" :close-on-content-click="false" :nudge-right="40" :return-value.sync="date1" lazy>
          <template v-slot:activator="{ on, attrs }">
            <v-text-field v-model="date1" label="Fecha máxima para dar acuso de recibido (si aplica)" prepend-icon="mdi-calendar" readonly v-bind="attrs" v-on="on"></v-text-field>
          </template>
          <v-date-picker v-model="date1" no-title scrollable>
            <v-spacer></v-spacer>
            <v-btn text color="primary" @click="menu1 = false">Cancel</v-btn>
            <v-btn text color="primary" @click="$refs.menu1.save(date1)">OK</v-btn>
          </v-date-picker>
        </v-menu>
      </v-col>
      <v-col cols="12" sm="6">
        <v-menu ref="menu2" v-model="menu2" :close-on-content-click="false" :nudge-right="40" :return-value.sync="date2" lazy>
          <template v-slot:activator="{ on, attrs }">
            <v-text-field v-model="date2" label="Fecha máxima para dar respuesta" prepend-icon="mdi-calendar" readonly v-bind="attrs" v-on="on"></v-text-field>
          </template>
          <v-date-picker v-model="date2" no-title scrollable>
            <v-spacer></v-spacer>
            <v-btn text color="primary" @click="menu2 = false">Cancel</v-btn>
            <v-btn text color="primary" @click="$refs.menu2.save(date2)">OK</v-btn>
          </v-date-picker>
        </v-menu>
      </v-col>
    </v-row>
  </v-card-text>
</v-card>
<v-card>
          <v-card-title>Descripción</v-card-title>
          <v-card-text>
            <strong>Aviso:</strong>
            <v-textarea v-model="requestDetail" outlined rows="5"></v-textarea>
          </v-card-text>
        </v-card>
        <v-card>
          <v-card-title>Evidencias que se anexan *si aplican*</v-card-title>
          <v-card-text>
            <strong></strong> 
            <v-textarea v-model="evidenciesDetail" outlined rows="5"></v-textarea>
          </v-card-text>
        </v-card>
        <v-card>
          <v-card-title>Investigacion realizada</v-card-title>
          <v-card-text>
            <strong></strong> 
            <v-textarea v-model="investigationDetail" outlined rows="5"></v-textarea>
          </v-card-text>
        </v-card>
        <v-card>
          <v-card-title>Acciones a tomar</v-card-title>
          <v-card-text>
            <strong></strong> 
            <v-textarea v-model="actionDetail" outlined rows="5"></v-textarea>
          </v-card-text>
        </v-card>
        <v-card>
          <v-card-title>Conclusiones</v-card-title>
          <v-card-text>
            <strong></strong> 
            <v-textarea v-model="conclusionDetail" outlined rows="5"></v-textarea>
          </v-card-text>
        </v-card>
        <v-card>
        <v-card-title>¿Amerita una acción correctiva o preventiva?</v-card-title>
        <v-container grid>
          <v-row>
            <v-col cols="12" sm="6">
              <v-checkbox v-model="actionYes" label="Si"></v-checkbox>
            </v-col>
            <v-col cols="12" sm="6">
              <v-checkbox v-model="actionNo" label="No"></v-checkbox>
            </v-col>
          </v-row>
        </v-container>
      </v-card>
      <v-row>
        <v-col cols="12">
          <v-btn @click="submitForm">Enviar</v-btn>
        </v-col>
      </v-row>
    </v-container>
  </v-form>

</template>

<script>
import Swal from 'sweetalert2'
export default {
  data() {
    return {
      firstName: '',
      lastName: '',
      idNumber: '',
      expedition: '',
      address: '',
      phone: '',
      email: '',
      complaint: false,
      appeal: false,
      writtenResponse: false,
      emailResponse: false,
      requestDetail: '',
    };
  },
  methods: {
        submitForm() {
          Swal.fire({
      title: 'Radicación',
      text: 'La radicación del presente documento lo puede hacer a través de correo certificado a la dirección Avenida 30 de agosto No. 30-23 Pereira - Risaralda o en la ventanilla única de radicación en las instalaciones del CDA CERTI EXPRESS PEREIRA S.A.S; en la dirección referenciada anteriormente.',
      icon: 'warning',
      showCancelButton: true,
      confirmButtonColor: '#3085d6',
      cancelButtonColor: '#d33',
      cancelButtonText: 'Cancelar',
      confirmButtonText: 'Acepto',
      customClass: {
        confirmButton: 'btn-custom-class',
        cancelButton: 'btn-custom-class2'
      }
    }).then((result) => {
      if (result.isConfirmed) {
        Swal.fire(
          'Recibido',
          'Tu solicitud ha sido procesada',
          'success'
        )
      }
    })
      
    },
  },
};
</script>

<style>

.btn-custom-class {
  font-size: 58px;
  padding: 10px 64px;
}

.btn-custom-class2 {
  font-size: 58px;
  padding: 10px 64px;
}

</style>

