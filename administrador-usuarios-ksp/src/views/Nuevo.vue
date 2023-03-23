<template>
    <div>
      <Header />
        <div class="container">
            <form action="" class="form-horizontal">
                <div class="form-group left row">
                  <div class="col">
                        <label for="" class="control-label col-sm-3">Nombre</label>
                        <div class="col-sm-7">
                            <input type="text" class="form-control" name="nombre" id="nombre" v-model="form.nombre">
                        </div>
                        <label for="" class="control-label col-sm-5">Apellido Paterno</label>
                          <div class="col-sm-7">
                              <input type="text" class="form-control" name="apaterno" id="apaterno" v-model="form.apaterno">
                          </div>
                        <label for="" class="control-label col-sm-5">Apellido Materno</label>
                          <div class="col-sm-7">
                              <input type="text" class="form-control" name="amaterno" id="amaterno" v-model="form.amaterno">
                          </div>
                    </div>
                    <div class="col">
                      <img :src="form.imagen"  class="form-control" alt="Imagen" style="width: 200px; height: 200px;"  />
                      <input type="file" class="control-label col-sm-7" id="input-file" @change="onFileChange">
                    </div>
                </div>
                <div class="form-group left row">
                     <div class="col">
                        <label for="" class="control-label col-sm-5">Usuario</label>
                        <div class="col-sm-7">
                            <input type="text" class="form-control" name="username" id="username" v-model="form.username">
                        </div>
                      </div>
                     <div class="col">
                      <label for="" class="control-label col-sm-5">Contraseña</label>
                        <div class="col-sm-7">
                            <input  type="password"  class="form-control" name="userpass" id="userpass" v-model="form.userpass">
                        </div>
                    </div>
                </div>
                <div class="form-group left row">
                     <div class="col">
                        <label for="" class="control-label col-sm-5">Sexo</label>
                        <div class="col-sm-7">
                        <select class="form-control" name="id_sexo" id="id_sexo" v-model="form.id_sexo">
                          <option value="0">Femenino</option>
                          <option value="1">Masculino</option>
                        </select>
                      </div>
                      </div>
                     <div class="col">
                          <label for="" class="control-label col-sm-5">Puesto</label>
                          <div class="col-sm-7">
                            <select class="form-control" name="id_puesto" id="id_puesto" v-model="form.id_puesto">
                              <option value="1">Interno</option>
                              <option value="2">Extenro</option>
                              <option value="3">Analista</option>
                              <option value="4">Gerente</option>
                            </select>
                          </div>
                    </div>
                </div>
                <div class="form-group left row">
                     <div class="col">
                        <label for="" class="control-label col-sm-5">Estatus</label>
                        <div class="col-sm-7">
                        <select class="form-control" name="id_estatus" id="id_estatus" v-model="form.id_estatus">
                          <option value="1">Activo</option>
                          <option value="1">Inactivo</option>
                        </select>
                      </div>
                      </div>
                     <div class="col">
                          <label for="" class="control-label col-sm-5">Permisos</label>
                          <div class="col-sm-7">
                            <select class="form-control" name="id_permiso" id="id_permiso" v-model="form.id_permiso">
                          <option value="1">Sin Permisos</option>
                          <option value="2">Consulta</option>
                          <option value="3">Escritura</option>
                          <option value="4">Administrador</option>
                        </select>
                          </div>
                    </div>
                </div>
                <div class="form-group left row">
                     <div class="col">
                        <label for="" class="control-label col-sm-5">Suledo</label>
                        <div class="col-sm-7">
                            <input type="text" class="form-control" name="sueldo" id="sueldo" v-model="form.sueldo">
                        </div>
                      </div>
                     <div class="col">
                      <label for="" class="control-label col-sm-5">Fecha Contrato</label>
                        <div class="col-sm-7">
                          <datepicker v-model="form.fecha_contrato" :format="customDateFormat"></datepicker>
                        </div> 
                    </div>
                </div>
                <div class="form-group left row">
                     <div class="col">
                        <div class="col-sm-7">
                          <button type="button" class="btn btn-primary margen col-sm-7" v-on:click="crear()" >Crear</button>
                        </div>
                      </div>
                </div>
                <div class="form-group left row">
                      <div class="col">
                        <div class="col-sm-7">
                          <button type="button" class="btn btn-dark margen col-sm-7" v-on:click="salir()"  >Salir</button>
                        </div>
                      </div> 
                </div> 
                <div class="alert alert-danger" role="alert" v-if="error">
                  {{error_msg}}
                </div>
            </form>
        </div>
      <!-- <Footer />   -->
    </div>

</template>
<script>

import Header from '@/components/Header.vue';
//import Footer from '@/components/Footer.vue';
import axios from 'axios';
import Datepicker from 'vuejs-datepicker';

export default {
name:"Editar",
components:{
Header,
Datepicker,
//Footer
},
data:function(){
return {
  form: {
        'username': "",
        'userpass': "",
        'id_puesto': "",
        'id_permiso': "",
        'id_estatus': "",
        'nombre': "",
        'apaterno': "",
        'amaterno': "",
        'sueldo': "",
        'id_sexo': "",
        'fecha_contrato': "",
        'imagen': "",
        'usuario_creacion': "",
        
      },
      base64Image: "",
      imageSrc: "",
      error: false,
      error_msg: "",
      usuarioactual:"",

}
},
methods:{
  crear(){
      axios.post("https://fvylzxqqv6.execute-api.us-east-1.amazonaws.com/dev/alta_persona",this.form)
      .then( data =>{
          let responseObj = JSON.parse(data.data);
          let arreglo = responseObj.response[0];
          let response = arreglo[0];
          let Status = response.Status;
          if (Status == 200){
            this.$router.push({ 
              name: 'Dashboard', 
              params: { 
                userId: this.usuarioactual
              } 
            });
          }else{
            this.error = true;
            this.error_msg = "Ocurrio un error al Crear Usuario"
          }
      })
  },
  salir(){
    this.$router.push({ 
              name: 'Dashboard', 
              params: { 
                userId: this.usuarioactual
              } 
            });
  },
  eliminar(){
    var enviar = {
        "pacienteId" : this.form.pacienteId,
        "token" : this.form.token
    };
    axios.delete("http://solodata.es/pacientes", { headers : enviar})
    .then( datos => {
        console.log(datos);
       this.$router.push("/dashboard");
    });
  },
  onFileChange(event) {
    const file = event.target.files[0];
    const reader = new FileReader();
    reader.readAsDataURL(file);
    reader.onload = () => {
      this.form.imagen = reader.result;
    };
},
},
mounted:function(){
    this.usuarioactual = this.$route.params.userId;
    this.form.usuario_creacion = this.usuarioactual
},
computed: {
customDateFormat() {
  return 'yyyy-MM-dd';
}
},
}
</script>
<style scoped>
.left{
text-align: left;
};
.margen{
margin-left: 15px;
margin-right: 15px;;
}
</style>