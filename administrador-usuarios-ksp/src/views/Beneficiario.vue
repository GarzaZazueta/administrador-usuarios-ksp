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
                    </div>
                    <div class="col">
                      <label for="" class="control-label col-sm-5">Apellido Paterno</label>
                        <div class="col-sm-7">
                            <input type="text" class="form-control" name="apaterno" id="apaterno" v-model="form.apaterno">
                        </div>
                    </div>
              </div>
              <div class="form-group left row">
                   <div class="col">
                    <label for="" class="control-label col-sm-5">Apellido Materno</label>
                        <div class="col-sm-7">
                            <input type="text" class="form-control" name="amaterno" id="amaterno" v-model="form.amaterno">
                        </div>
                    </div>
                    <div class="col">
                      <label for="" class="control-label col-sm-5">Persona ID</label>
                      <div class="col-sm-7">
                        <input type="text" class="form-control" name="id_persona" id="id_persona" v-model="form.id_persona" readonly>
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
                    <label for="" class="control-label col-sm-5">Parentesco</label>
                      <div class="col-sm-7">
                        <select class="form-control" name="id_parentesco" id="id_parentesco" v-model="form.id_parentesco">
                          <option value="0">Hermano</option>
                          <option value="1">Conyuge</option>
                        </select>
                      </div>
                    </div>
              </div>
              <div class="form-group left row">
                   <div class="col">
                    <label for="" class="control-label col-sm-5">Fecha de Nacimiento</label>
                      <div class="col-sm-7">
                        <datepicker v-model="form.fechanacimiento" :format="customDateFormat"></datepicker>
                      </div> 
                  </div>
              </div>
              <div class="form-group left row">
                   <div class="col">
                      <div class="col-sm-7">
                        <button type="button"  id="btnEditar" class="btn btn-primary margen col-sm-11" v-on:click="editar()" >Actualizar</button>
                      </div>
                    </div>
                   <div class="col">
                      <div class="col-sm-7">
                        <button type="button"  id="btnEliminar" class="btn btn-danger margen col-sm-11" v-on:click="eliminar()" >Eliminar</button>
                      </div> 
                  </div>
              </div>
              <div class="form-group left row">
                   <div class="col">
                      <div class="col-sm-7">
                        <button type="button" id="btnAlta" class="btn btn-success margen col-sm-11" v-on:click="guardar()"  >Agregar Beneficiario</button>
                      </div>
                    </div> 
                    <div class="col">
                      <div class="col-sm-7">
                        <button type="button" class="btn btn-dark margen col-sm-11" v-on:click="salir()"  >Salir</button>
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
      'id_persona': "",
      'nombre': "",
      'apaterno': "",
      'amaterno': "",
      'id_parentesco': "",
      'id_beneficiario': "",
      'id_sexo': "",
      'fechanacimiento': "",
      "usuario_modificacion":"",
      "usuario_creacion":"",
    },
    base64Image: "",
    imageSrc: "",
    error: false,
    error_msg: "",
}
},
methods:{
editar(){
    axios.put("https://fvylzxqqv6.execute-api.us-east-1.amazonaws.com/dev/actualiza_beneficiario",this.form)
    .then( data =>{
        let responseObj = JSON.parse(data.data);
        let arreglo = responseObj.response[0];
        let response = arreglo[0];
        let Status = response.status;
        if (Status == 200){
          this.$router.push({ 
          name: 'Dashboard', 
          params: { 
            userId: this.form.usuario_modificacion
          } 
        });
        }else{
          this.error = true;
          this.error_msg = "Ocurrio un error al actualizar"
        }
    })
},
salir(){
  this.$router.push({ 
        name: 'Dashboard', 
        params: { 
          userId: this.form.usuario_modificacion
        } 
      });
},
guardar(){
  axios.post("https://fvylzxqqv6.execute-api.us-east-1.amazonaws.com/dev/alta_beneficiario",this.form)
    .then( data =>{
        let responseObj = JSON.parse(data.data);
        let arreglo = responseObj.response[0];
        let response = arreglo[0];
        let Status = response.status;
        if (Status == 200){
          this.$router.push({ 
          name: 'Dashboard', 
          params: { 
            userId: this.form.usuario_modificacion
          } 
        });
        }else{
          this.error = true;
          this.error_msg = "Ocurrio un error al actualizar"
        }
    })
},
eliminar(){
  var enviar = {
      "id_beneficiario" : this.form.id_beneficiario    
  };
  axios.post("https://fvylzxqqv6.execute-api.us-east-1.amazonaws.com/dev/elimina_beneficiario",enviar)
  .then( datos => {
              let responseObj = JSON.parse(datos.data);
              let arreglo = responseObj.response[0];
              let response = arreglo[0];
              let Status = response.status;
              if (Status == 200){
                this.$router.push({ 
                name: 'Dashboard', 
                params: { 
                  userId: this.form.usuario_modificacion
                } 
              });
              }else{
                this.error = true;
                this.error_msg = "Ocurrio un error al actualizar"
              }
  });
},
},
mounted:function(){
this.form.id_persona = this.$route.params.persona;
this.form.usuario_modificacion = this.$route.params.usuarioactual;
this.form.usuario_creacion = this.$route.params.usuarioactual;
console.log(this.$route.params.persona)
console.log(this.$route.params.usuarioactual)
this.imageSrc = this.base64Image;
let json = {
  "id_persona": this.form.id_persona
};

axios.post("https://fvylzxqqv6.execute-api.us-east-1.amazonaws.com/dev/obtiene_beneficiario",json)
.then( datos => {

    let responseObj = JSON.parse(datos.data);
    console.log(responseObj)
    if(responseObj.response[0] == null){
      console.log("no hay beneficiarios")
      let botoneditar = document.getElementById("btnEditar");
      botoneditar.setAttribute("disabled", "disabled");
      let botoneliminar = document.getElementById("btnEliminar");
      botoneliminar.setAttribute("disabled", "disabled");

    }else{
      let botonalta = document.getElementById("btnAlta");
      botonalta.setAttribute("disabled", "disabled");
      let arreglo = responseObj.response[0];
      console.log(responseObj)
      let user = arreglo[0];
      this.form.nombre = user.nombre;
      this.form.apaterno = user.apaterno;
      this.form.amaterno = user.amaterno;
      this.form.id_sexo = user.id_sexo;
      this.form.id_parentesco = user.id_parentesco;
      this.form.id_persona = user.id_persona;
      this.form.fechanacimiento = user.fechanacimiento;
      this.form.id_beneficiario = user.id_beneficiario;
    }   
})

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