<!-- Script JS -->
<script>
  import * as d3 from 'd3'
  import { onMount } from 'svelte';

  let mostrarMenu = false;
  let tituloAgrandado = false;
  let imagenSeleccionada = null;

  function verImagen(src) {
    imagenSeleccionada = src;
  }
  function cerrarImagen() {
    imagenSeleccionada = null;
  }

  function cerrarSiEscape(event) {
    if (event.key === 'Escape') cerrarImagen();
  }

  onMount(() => {
    window.addEventListener('keydown', cerrarSiEscape);
    const flourishScript = document.createElement('script');
    flourishScript.src = 'https://public.flourish.studio/resources/embed.js';
    flourishScript.async = true;
    document.body.appendChild(flourishScript);
    return () => {
      window.removeEventListener('keydown', cerrarSiEscape);
      document.body.removeChild(flourishScript);
    };
  });

  let data = [
    { valor: 24, anio: 1971, campaña: "I'd Like to Buy the World a Coke", inversion: 15 },
    { valor: 33, anio: 1985, campaña: "New Coke", inversion: 28 },
    { valor: 42, anio: 1993, campaña: "Always Coca-Cola", inversion: 35 },
    { valor: 54, anio: 2000, campaña: "Coca-Cola Music", inversion: 32 },
    { valor: 63, anio: 2006, campaña: "The Coke Side of Life", inversion: 50 },
    { valor: 71, anio: 2010, campaña: "Open Happiness", inversion: 44 },
    { valor: 77, anio: 2014, campaña: "Share a Coke", inversion: 38 },
    { valor: 87, anio: 2018, campaña: "Taste the Feeling", inversion: 41 },
    { valor: 92, anio: 2020, campaña: "Together Tastes Better", inversion: 55 },
    { valor: 98, anio: 2024, campaña: "Real Magic", inversion: 60 },
  ]
  let colorGradiente = d3.scaleLinear()
    .domain([0, 0.5, 1])
    .range(['#ff3c3c', '#ec1d1d', '#770000']);

  let altura = d3
    .scaleLinear()
    .domain([24, 98])
    .range([100, 300])
</script>

<!-- Estructura contenido HTML -->
<main id="container">
  <div class="menu-wrapper fixed-menu">
    <button class="hamburger" on:click={() => mostrarMenu = !mostrarMenu}>
      ☰
    </button>
  
    {#if mostrarMenu}
      <nav class="menu-desplegable">
        <ul>
          <li><a href="#grafico-principal">Visualización principal</a></li>
          <li><a href="#ventas">Ventas</a></li>
          <li><a href="#inversion">Inversión</a></li>
          <li><a href="#comparativa">Comparativa</a></li>
        </ul>
      </nav>
    {/if}
  </div>  
  <div class='content'>
    <header>
      <h1 class="title">Coca-Cola</h1>
      <h2 class="description">La evolución de una marca icónica: Coca-Cola a través de sus campañas</h2>  
      <h3 class="text_1">
        Coca-Cola es un gesto, un recuerdo, un instante que marcó una época. 
        Desde un jingle que unió al mundo hasta una botella compartida entre amigos, 
        cada campaña encierra mucho más que marketing: cuenta una parte de nuestra cultura. 
      </h3> 
    </header>

    <section id="grafico-principal">
      <div class="chart">
        <div class="visual-array">
          {#each data as d, i}
          <div class="item">
            <div class="etiquetas-superiores">
              <p class="anio-top">{d.anio}</p>
              <p class="campania-nombre">{d.campaña}</p>
            </div>
            <div class="imagen-wrapper">
              {#if d.valor < 70}
                <button class="image-button" on:click={() => verImagen('/images/Lata.svg')}>
                  <img src="/images/Lata.svg" alt="Lata de Coca-Cola" style="height: {altura(d.valor)}px;" />
                </button>
              {:else}
                <button class="image-button" on:click={() => verImagen('/images/Botella.svg')}>
                  <img src="/images/Botella.svg" alt="Botella de Coca-Cola" style="height: {altura(d.valor)}px;" />
                </button>
              {/if}
            </div>
            <p class="num" style="color: {colorGradiente(i / (data.length - 1))};">{d.valor}</p>
          </div>
          {/each}
        </div>
      </div>
      <p class="leyenda">
        El tamaño de cada envase, junto a cada número, reflejan el impacto simbólico dentro de la evolución publicitaria de la marca.
      </p> 
    </section>

    <h3 class="text_2">
      En los últimos años Coca-Cola no solo vendió bebidas. Vendió momentos, experiencias y una identidad compartida por millones que marcaron generaciones.  <br>
      Esta visualización recorre diez de las campañas publicitarias más significativas de la empresa junto a la inversion correspondiente en millones de dólares. Dichas campañas dejaron huella por su creatividad, pero a su vez por cómo quedaron impresas en la memoria colectiva.  
      Desde la icónica melodía de los años 70 hasta los nombres impresos personalizados que generaron tendencia en los 2010s. <br> 
      Cada envase representa el impacto simbólico de una campaña, cuenta algo más que un año: cuenta cómo nos conectamos con una marca que es parte de nuestra vida cotidiana.
    </h3> 

    <section id="ventas">
      <h2 class="titulo-flourish">Gráfico de Área</h2>
      <div class="grafico-flourish">
        <iframe src='https://flo.uri.sh/visualisation/22635724/embed' title='Interactive or visual content' class='flourish-embed-iframe' frameborder='0' scrolling='no' style='width:100%;height:450px;' sandbox='allow-same-origin allow-forms allow-scripts allow-downloads allow-popups allow-popups-to-escape-sandbox allow-top-navigation-by-user-activation'></iframe>
      </div>
      <p class="grafico-descripcion">
        El impacto de las campañas de Coca-Cola muestran una curva de crecimiento firme durante la última década, incluso atravesando contextos desafiantes como la pandemia. Esto refleja no solo una marca fuerte, sino una capacidad de adaptación que conecta con su audiencia.
      </p>
    </section>
    
    <section id="inversion">
      <h2 class="titulo-flourish">Gráfico Combinado</h2>
      <div class="grafico-flourish">
        <iframe src='https://flo.uri.sh/visualisation/22657098/embed' title='Interactive or visual content' class='flourish-embed-iframe' frameborder='0' scrolling='no' style='width:100%;height:450px;' sandbox='allow-same-origin allow-forms allow-scripts allow-downloads allow-popups allow-popups-to-escape-sandbox allow-top-navigation-by-user-activation'></iframe>
      </div>
      <p class="grafico-descripcion">
        Esta visualización destaca cómo las decisiones de inversión en campañas publicitarias impactaron el rendimiento simbólico de cada año. Los picos en inversión —como en 2018 y 2020— coinciden con momentos de fuerte crecimiento, revelando la estrecha relación entre creatividad, inversión y resultados.
      </p>
    </section>
    
    <section id="comparativa">
      <h2 class="titulo-flourish">Gráfico de Línea</h2>
      <div class="grafico-flourish">
        <iframe src='https://flo.uri.sh/visualisation/22645584/embed' title='Interactive or visual content' class='flourish-embed-iframe' frameborder='0' scrolling='no' style='width:100%;height:450px;' sandbox='allow-same-origin allow-forms allow-scripts allow-downloads allow-popups allow-popups-to-escape-sandbox allow-top-navigation-by-user-activation'></iframe>
      </div>
      <p class="grafico-descripcion">
        A través de líneas se puede observar con claridad la diferencia entre inversión e impacto a lo largo del tiempo. Lejos de una caída, los últimos años muestran un alto impacto publicitario, incluso mayor al de años con menor inversión. Esto sugiere que Coca-Cola ha logrado afinar sus estrategias para maximizar resultados
      </p>
    </section>

    <table class="tabla-campanias">
      <thead>
        <tr>
          <th>Campaña</th>
          <th>Año</th>
          <th>Impacto</th>
          <th>Inversión</th>
        </tr>
      </thead>
      <tbody>
        {#each data as d, i}
          <tr>
            <td>{d.campaña}</td>
            <td style="color: {colorGradiente(i / (data.length - 1))}; font-weight: bold; text-align: center;">
              {d.anio}
            </td>
            <td style="text-align: center;">{d.valor}</td>
            <td style="text-align: center;">{d.inversion}</td>
          </tr>
        {/each}
      </tbody>
    </table>
  </div>

{#if imagenSeleccionada}
  <div
    class="modal"
    role="button"
    tabindex="0"
    aria-label="Cerrar imagen ampliada"
    on:click={cerrarImagen}
    on:keydown={(e) => e.key === 'Escape' && cerrarImagen()}
  >
    <button
      class="modal-img-button"
      aria-label="Imagen ampliada"
      on:click|stopPropagation
    >
      <img
        src={imagenSeleccionada}
        alt="Imagen ampliada"
        class="modal-img"
      />
    </button>
  </div>
{/if}

</main>

<footer class="footer">
  <p>
    Proyecto realizado por <strong>Melina Dborkin</strong>  
    | Estudiante de <strong>Tecnología Digital</strong> en la 
    <strong>Universidad Torcuato Di Tella</strong>  
    | Buenos Aires, Argentina  
    | <a href="mailto:mdborkin@mail.utdt.edu">mdborkin@mail.utdt.edu</a>  
    | <a href="https://www.linkedin.com/in/melinadborkin" target="_blank">LinkedIn</a>
  </p>
</footer>