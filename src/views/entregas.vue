<template>
  <div class="container">
    <div class="header-batoi offset-3 col-6 col-lg-12 offset-lg-0 justify-content-center">
      <b-button class="btn-red" @click="logout()"
        ><a>Log-out <b-icon-box-arrow-right></b-icon-box-arrow-right></a
      ></b-button>
    </div>
    <div class="col-12" id="entregas">
      <table class="table table-stripped table-hover table-repartidor">
        <thead>
          <tr>
            <th>ID del Pedido</th>
            <th>Destinatario</th>
            <th>Estado de la Entrega</th>
            <th>Fecha de entrega</th>
            <th>Acciones</th>
          </tr>
        </thead>
        <tbody>
          <!--Aquí encontraremos todos los pedidos-->
          <order-item
            v-for="order in orders"
            :key="order.id"
            :order="order"
          ></order-item>
        </tbody>
      </table>
    </div>
  </div>
</template>

<script>
import orderItem from "../components/orderItem";

export default {
  components: {
    orderItem,
  },
  mounted() {
    // comprobar la promesa porque sigue dando los errores al montar la tabla
    this.$store.dispatch("loadStates").then(() => {
      this.$store.dispatch("enRutaState");
      this.$store.dispatch("loadOrders");
    });
  },
  computed: {
    orders() {
      let orders = this.$store.state.orders;
      orders.sort((a, b) => (a.order < b.order ? -1 : +1));
      return orders;
    },
  },
  methods: {
    logout(){
      localStorage.removeItem('token');
      this.$router.push('/login');

    }

  },
};
</script>
