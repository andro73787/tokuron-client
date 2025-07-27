<script>
  import { Router } from '@mateothegreat/svelte5-router';
  import Register from "./routes/Register.svelte";

  const routes = [
    {
      name: 'register',
      path: '/register',
      component: Register,
    }
  ];
</script>

<Router {routes} />