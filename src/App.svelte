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

  function cerrarSiClickAfuera(event) {
  const menu = document.querySelector('.menu-desplegable');
  const boton = document.querySelector('.hamburger');
  
  if (mostrarMenu && menu && !menu.contains(event.target) && !boton.contains(event.target)) {
    mostrarMenu = false;
  }
}

  onMount(() => {
  window.addEventListener('keydown', cerrarSiEscape);
  window.addEventListener('click', cerrarSiClickAfuera);

  const flourishScript = document.createElement('script');
  flourishScript.src = 'https://public.flourish.studio/resources/embed.js';
  flourishScript.async = true;
  document.body.appendChild(flourishScript);

  return () => {
    window.removeEventListener('keydown', cerrarSiEscape);
    window.removeEventListener('click', cerrarSiClickAfuera);
    document.body.removeChild(flourishScript);
  };
});

  let data = [
    { valor: 24, anio: 1971, campaña: "I'd Like to Buy the World a Coke", inversion: 23 },
    { valor: 33, anio: 1985, campaña: "New Coke", inversion: 35 },
    { valor: 42, anio: 1993, campaña: "Always Coca-Cola", inversion: 45 },
    { valor: 54, anio: 2000, campaña: "Coca-Cola Music", inversion: 56 },
    { valor: 63, anio: 2006, campaña: "The Coke Side of Life", inversion: 50 },
    { valor: 71, anio: 2010, campaña: "Open Happiness", inversion: 43 },
    { valor: 77, anio: 2014, campaña: "Share a Coke", inversion: 38 },
    { valor: 87, anio: 2018, campaña: "Taste the Feeling", inversion: 32 },
    { valor: 92, anio: 2020, campaña: "Together Tastes Better", inversion: 28 },
    { valor: 98, anio: 2024, campaña: "Real Magic", inversion: 25 },
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
          <li><a href="#home">Home</a></li>
          <li><a href="#grafico-principal">Visualización principal</a></li>
          <li><a href="#Hallazgo clave">Hallazgo clave</a></li>
          <li><a href="#Impacto">Impacto</a></li>
          <li><a href="#Reglas del éxito">Reglas del éxito</a></li>
          <li><a href="#Datos">Datos</a></li>
        </ul>
      </nav>
    {/if}
  </div>  
  <div class='content'>
    <section id="home">
      <header>
        <h1 class="title">Coca-Cola</h1>
        <h2 class="description">La evolución de una marca icónica</h2>  
        <h3 class="text_1">
          Coca-Cola es un gesto, un recuerdo, un instante que marcó una época. 
          Desde un jingle que unió al mundo hasta una botella compartida entre amigos, 
          cada campaña encierra mucho más que marketing: cuenta una parte de nuestra cultura. 
        </h3> 
      </header>
  
      <div class="imagen-tipo">
        <img src="/images/LataTipo.svg" alt="Lata tipo" style="height: {altura(40)}px;"/>
        <img src="/images/BotellaTipo.svg" alt="Botella tipo" style="height: {altura(63)}px;"/>
      </div>
      <p class="leyenda">
        Las latas representan magnitudes menores a 70. Las botellas representan magnitudes mayores a 70. 
        El tamaño de cada envase, junto a cada número, reflejan el impacto simbólico dentro de la evolución publicitaria de la marca o el moto invertido en millones de USD.
      </p> 
      <hr class="linea-roja1" />
    </section>

    <section id="grafico-principal">
      <div class="chart">
        <h2 class="titulo-visualizacion">Impacto porcentual de las campañas publicitarias en las ventas de Coca-Cola</h2>
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
        
        <h2 class="titulo-visualizacion">Inversión de las campañas publicitarias en las ventas de Coca-Cola en millones de dólares</h2>
        <div class="visual-array">
          {#each data as d, i}
          <div class="item">
            <div class="etiquetas-superiores">
              <p class="anio-top">{d.anio}</p>
              <p class="campania-nombre">{d.campaña}</p>
            </div>
            <div class="imagen-wrapper">
              {#if d.inversion < 70}
                <button class="image-button" on:click={() => verImagen('/images/Lata2.svg')}>
                  <img src="/images/Lata2.svg" alt="Lata de Coca-Cola" style="height: {altura(d.inversion)}px;" />
                </button>
              {:else}
                <button class="image-button" on:click={() => verImagen('/images/BotellaTipo.svg')}>
                  <img src="/images/Botella.svg" alt="Botella de Coca-Cola" style="height: {altura(d.inversion)}px;" />
                </button>
              {/if}
            </div>
            <p class="num" style="color: {colorGradiente(i / (data.length - 1))};">{d.inversion}</p>
          </div>
          {/each}
        </div>
      </div>
    </section>


    <hr class="linea-roja2" />

    <h3 class="text_2">
      En los últimos años Coca-Cola no solo vendió bebidas. Vendió momentos, experiencias y una identidad compartida por millones que marcaron generaciones.  <br>
      Esta visualización recorre diez de las campañas publicitarias más significativas de la empresa junto a la inversion correspondiente en millones de dólares. Dichas campañas dejaron huella por su creatividad, pero a su vez por cómo quedaron impresas en la memoria colectiva.  
      Desde la icónica melodía de los años 70 hasta los nombres impresos personalizados que generaron tendencia en los 2010s. <br> 
      Cada envase representa el impacto simbólico de una campaña, cuenta algo más que un año: cuenta cómo nos conectamos con una marca que es parte de nuestra vida cotidiana.
    </h3> 


    <hr class="linea-roja2" />

    <section id="Hallazgo clave">
      <h2 class="titulo-visualizacion">Hallazgo clave: la cantidad de dinero invertida en una campaña no garantiza su impacto simbólico.</h2>
      <div class="grafico-flourish">
        <iframe src='https://flo.uri.sh/visualisation/22635724/embed' title='Interactive or visual content' class='flourish-embed-iframe' frameborder='0' scrolling='no' style='width:100%;height:450px;' sandbox='allow-same-origin allow-forms allow-scripts allow-downloads allow-popups allow-popups-to-escape-sandbox allow-top-navigation-by-user-activation'></iframe>
      </div>
      <p class="grafico-descripcion">
        En las primeras décadas, Coca-Cola parecía seguir una lógica directa: a mayor inversión en campañas, mayor impacto simbólico. 
        Sin embargo, a partir de 2018, la curva se desvía. A medida que la inversión disminuye, el impacto publicitario sigue creciendo de manera sostenida. 
        Esto revela un cambio fundamental: la marca dejó de depender únicamente del presupuesto y comenzó a apoyarse en estrategias creativas más profundas y en la conexión emocional con su audiencia.
      </p>
    </section>
    
    <section id="Impacto">
      <h2 class="titulo-visualizacion">Menos dinero, más impacto: el nuevo paradigma de Coca-Cola</h2>
      <div class="grafico-flourish">
        <iframe src='https://flo.uri.sh/visualisation/22657098/embed' title='Interactive or visual content' class='flourish-embed-iframe' frameborder='0' scrolling='no' style='width:100%;height:600px;' sandbox='allow-same-origin allow-forms allow-scripts allow-downloads allow-popups allow-popups-to-escape-sandbox allow-top-navigation-by-user-activation'></iframe>
      </div>
      <p class="grafico-descripcion">
        Este gráfico muestra cómo, especialmente en los últimos años, la efectividad simbólica de las campañas de Coca-Cola supera ampliamente la inversión monetaria realizada. 
        El éxito ya no reside en cuánto se invierte, sino en cómo se transmite un mensaje poderoso. 
        A partir de 2018, la brecha entre inversión e impacto se amplía: con presupuestos más moderados, Coca-Cola logra niveles de resonancia cultural más altos que nunca.
      </p>
    </section>

    <section id="Reglas del éxito">
      <h2 class="titulo-visualizacion">Más impacto, menos inversión: Coca-Cola redefine las reglas del éxito</h2>
      <div class="grafico-flourish">
        <iframe src='https://flo.uri.sh/visualisation/22884297/embed' title='Interactive or visual content' class='flourish-embed-iframe' frameborder='0' scrolling='no' style='width:100%;height:600px;' sandbox='allow-same-origin allow-forms allow-scripts allow-downloads allow-popups allow-popups-to-escape-sandbox allow-top-navigation-by-user-activation'></iframe>      
      <p class="grafico-descripcion">
        El scatter plot es una manera más de evidenciar la diferencia entre inversión e impacto a lo largo del tiempo. 
        La dispersión de los puntos —lejos de formar una línea recta— confirma que <strong>el éxito de Coca-Cola en los últimos años radicó más en la creatividad y la narrativa emocional que en la magnitud de su presupuesto publicitario</strong>.
      </p>
    </section>

    <section id="Datos">
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
    </section>
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
    Proyecto realizado por <strong>Melina Dborkin</strong> en la materia <strong>Visualización de Datos</strong> 
    | Estudiante de <strong>Tecnología Digital</strong> en la <strong>Universidad Torcuato Di Tella</strong> 
    <br> 
      <a href="mailto:mdborkin@mail.utdt.edu">mdborkin@mail.utdt.edu</a>  
    | <a href="https://www.linkedin.com/in/melinadborkin" target="_blank">LinkedIn</a>
  </p>
</footer>
