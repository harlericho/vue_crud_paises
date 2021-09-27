<template>
  <div class="container">
    <div class="row justify-content-center">
      <div class="col-md-3">
        <div class="card border-success mb-3" style="max-width: 30rem">
          <div
            class="card-header bg-transparent border-success"
            id="headerCard1"
          >
            Paises
          </div>
          <div class="card-body text-success">
            <h5 class="card-title">Formulario</h5>
            <form v-on:submit.prevent="guardarDatos()">
              <input type="hidden" v-model="paises.id" />
              <div class="mb-3">
                <label for="nombre" class="form-label">Nombre</label>
                <input
                  type="text"
                  class="form-control"
                  v-model="paises.nombre"
                  ref="nombre"
                  autofocus
                />
              </div>
              <div class="mb-3">
                <label for="nombres" class="form-label">Continente</label>
                <input
                  type="text"
                  class="form-control"
                  v-model="paises.continente"
                  ref="continente"
                />
              </div>
              <button type="submit" class="btn btn-primary" id="margen">
                Guardar
              </button>
              <button
                type="button"
                class="btn btn-info"
                v-on:click="crearNuevo()"
              >
                Nuevo
              </button>
            </form>
          </div>
          <div
            class="card-footer bg-transparent border-success"
            id="footerCard1"
          >
            @harlericho
          </div>
        </div>
      </div>
      <div class="col-md-9">
        <div class="card border-danger mb-3" style="max-width: 50rem">
          <div
            class="card-header bg-transparent border-danger"
            id="headerCard2"
          >
            Paises
          </div>
          <div class="card-body text-danger">
            <h5 class="card-title">Listado</h5>
            <div class="table table-responsive">
              <table class="table table-striped">
                <thead>
                  <tr id="cabezeraTable">
                    <th>#</th>
                    <th>Nombre</th>
                    <th>Continente</th>
                    <th colspan="2">Acciones</th>
                  </tr>
                </thead>
                <tbody>
                  <tr v-for="(listar, i) in listado" :key="listar.id">
                    <td scope="row">{{ i + 1 }}</td>
                    <td>{{ listar.nombre }}</td>
                    <td>{{ listar.continente }}</td>
                    <td>
                      <button
                        type="button"
                        class="btn btn-primary"
                        v-on:click="obtenerDatos(listar.id)"
                      >
                        Editar
                      </button>
                    </td>
                    <td>
                      <button
                        type="button"
                        class="btn btn-danger"
                        v-on:click="eliminarDatos(listar.id)"
                      >
                        Eliminar
                      </button>
                    </td>
                  </tr>
                </tbody>
              </table>
            </div>
          </div>
          <div
            class="card-footer bg-transparent border-danger"
            id="footerCard2"
          >
            @harlericho
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
const url = "http://127.0.0.1:5000/";
import axios from "axios";
import Swal from "sweetalert2";
export default {
  data() {
    return {
      paises: {
        id: "",
        nombre: "",
        continente: "",
      },
      listado: [],
    };
  },
  mounted() {
    this.listadoDatos();
  },
  methods: {
    listadoDatos() {
      axios
        .get(url)
        .then((res) => {
          this.listado = res.data.data;
          this.$toast.show("Listado de los datos", {
            position: "top-right",
            duration: 2000,
          });
        })
        .catch((err) => {
          console.error(err);
        });
    },
    guardarDatos() {
      const params = {
        nombre: this.paises.nombre,
        continente: this.paises.continente,
      };
      if (this.paises.id === "") {
        axios
          .post(url, params)
          .then((res) => {
            if (res.data.nombre) {
              this.$toast.error(res.data.nombre, {
                position: "top-right",
                duration: 2000,
              });
              this.$refs.nombre.focus();
            } else {
              this.$toast.success(res.data.data, {
                position: "top-right",
                duration: 2000,
              });
              this.listadoDatos();
              this.limpiar();
            }
          })
          .catch((err) => {
            console.error(err);
          });
      } else {
        let id = this.paises.id;
        axios
          .put(url + id, params)
          .then((res) => {
            if (res.data.nombre) {
              this.$toast.error(res.data.nombre, {
                position: "top-right",
                duration: 2000,
              });
              this.$refs.nombre.focus();
            } else {
              this.$toast.success(res.data.data, {
                position: "top-right",
                duration: 2000,
              });
              this.listadoDatos();
              this.limpiar();
            }
          })
          .catch((err) => {
            console.error(err);
          });
      }
    },
    obtenerDatos(id) {
      this.$toast.info("Usted va realizar una actualización", {
        position: "top-right",
        duration: 2000,
      });
      axios
        .get(url + id)
        .then((res) => {
          this.paises.id = res.data.data.id;
          this.paises.nombre = res.data.data.nombre;
          this.paises.continente = res.data.data.continente;
        })
        .catch((err) => {
          console.error(err);
        });
    },
    eliminarDatos(id) {
      Swal.fire({
        title: "Estás seguro?",
        text: "No podrás revertir esto!",
        icon: "warning",
        showCancelButton: true,
        confirmButtonColor: "#3085d6",
        cancelButtonColor: "#d33",
        confirmButtonText: "Si, eliminarlo!",
      }).then((result) => {
        if (result.isConfirmed) {
          Swal.fire("Eliminado!", "Su archivo ha sido eliminado", "success");
          axios
            .delete(url + id)
            .then((res) => {
              this.$toast.success(res.data.data, {
                position: "top-right",
                duration: 2000,
              });
              this.listadoDatos();
            })
            .catch((err) => {
              console.error(err);
            });
        }
      });
    },
    crearNuevo() {
      this.$toast.show("Crear nuevo registro", {
        position: "top-right",
        duration: 2000,
      });
      this.paises.id = "";
      this.paises.nombre = "";
      this.paises.continente = "";
      this.$refs.nombre.focus();
    },
    limpiar() {
      this.paises.id = "";
      this.paises.nombre = "";
      this.paises.continente = "";
      this.$refs.nombre.focus();
    },
  },
};
</script>

<style>
#margen {
  margin-right: 5%;
}
</style>