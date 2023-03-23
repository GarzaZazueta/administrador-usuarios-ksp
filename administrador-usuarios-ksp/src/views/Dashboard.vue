<template>
    <div>
      <Header />
      <div class="container izquierda">
        <button class="btn btn-primary margen" v-on:click="nuevo()">Nuevo Usuario</button>
        <button class="btn btn-success margen"  v-on:click="exportar()">Exportar en Excel</button>
        <br><br>
        <table class="table table-hover">
        <thead>
            <tr>
            <th scope="col">id_usuario</th>
            <th scope="col">username</th>
            <th scope="col">id_estatus</th>
            <th scope="col">nombre</th>
            <th scope="col">apaterno</th>
            <th scope="col"></th> <!-- nueva columna para botón de edición -->
            </tr>
        </thead>
        <tbody>
            <tr v-for="usuarios in paginatedList" :key="usuarios[0].id_persona">
            <th scope="row">{{ usuarios[0].id_usuario }}</th>
            <td>{{ usuarios[0].username }}</td>
            <td>{{ usuarios[0].id_estatus }}</td>
            <td>{{ usuarios[0].nombre }}</td>
            <td>{{ usuarios[0].apaterno }}</td>
            <td> <!-- columna para botón de edición -->
                <button class="btn btn-secondary" v-on:click="editar(usuarios[0].id_usuario)">Editar</button>
            </td>
            </tr>
        </tbody>
        </table>
        <b-pagination v-model="pagina" :total-rows="Listausuarios.length" :per-page="10"></b-pagination>
      </div>
      <button class="btn btn-danger margen"  v-on:click="salir()">Salir</button>
      <Footer />
    </div>
  </template>
  
  <script>
  import Header from "@/components/Header.vue";
  import Footer from "@/components/Footer.vue";
  import axios from "axios";
  import { BPagination } from "bootstrap-vue";
  import * as XLSX from 'xlsx';
  
  export default {
    name: "Dashboard",
    components: {
      Header,
      Footer,
      BPagination
    },
    data() {
      return {
        Listausuarios: [],
        pagina: 1,
        perPage: 10,
        usuarioactual:""
      };
    },
    computed: {
      paginatedList() {
        const start = (this.pagina - 1) * this.perPage;
        const end = start + this.perPage;
        return this.Listausuarios.slice(start, end);
      }
    },
    methods: {
      editar(id) {
        this.$router.push({ 
              name: 'Editar', 
              params: { 
                id_edit: id,
                userId: this.usuarioactual
              } 
            });
      },
      nuevo() {
        this.$router.push({ 
              name: 'Nuevo', 
              params: { 
                userId: this.usuarioactual 
              } 
            });
      },
      salir() {
        this.$router.push({ 
              name: 'Home'
            });
      },
      exportar() {
            console.log("Aqui Mero")
            let responseObj=""
            let data3 = [];

            let direccion = "https://fvylzxqqv6.execute-api.us-east-1.amazonaws.com/dev/get_info_exportar";
            axios.get(direccion).then(data => {
                responseObj = JSON.parse(data.data);
                data3 = responseObj.response.flat().map(persona => {
                return {
                    nombre: persona.nombre,
                    apellido: `${persona.apaterno} ${persona.amaterno}`,
                    sueldo: persona.sueldo
                };
                });

                console.log(data3);
            });
            const workbook = XLSX.utils.book_new();
            const sheet = XLSX.utils.json_to_sheet(data3);
            XLSX.utils.book_append_sheet(workbook, sheet, 'Hoja 1');
            const fecha = new Date().toLocaleDateString();
            const filename = `datos-${fecha}.xlsx`;
            XLSX.writeFile(workbook, filename);    
      }
    },
    mounted: function() {
      this.usuarioactual = this.$route.params.userId;
      let direccion = "https://fvylzxqqv6.execute-api.us-east-1.amazonaws.com/dev/obtiene_usuario";
      axios.get(direccion).then(data => {
        let responseObj = JSON.parse(data.data);
        this.Listausuarios = responseObj.response;
      });
    }
  };
  </script>
  
  <style scoped>
  .izquierda {
    text-align: left;
  }
  </style>
  