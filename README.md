# Netflix
Cuevaflix es un sitio web dedicado a informar sobre las próximas películas que llegarán a los cines. Nuestro objetivo es mantener a los amantes del cine actualizados con noticias, fechas de estreno, avances, curiosidades y novedades de las producciones más esperadas del año.
<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <!-- TRUCO PARA PRODUCCIÓN: Evita bloqueos de servidores externos al cargar imágenes -->
    <meta name="referrer" content="no-referrer">
    <title>Cuevaflix - Estrenos y Clásicos de Cine</title>
    <style>
        /* --- ESTILOS GENERALES Y CONFIGURACIÓN --- */
        :root {
            --primary-color: #E50914; 
            --bg-dark: #141414;
            --bg-sidebar: #080808;
            --bg-card: #181818;
            --text-main: #FFFFFF;
            --text-muted: #AAAAAA;
        }

        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
        }

        body {
            background-color: var(--bg-dark);
            color: var(--text-main);
            display: flex;
            min-height: 100vh;
            overflow-x: hidden;
        }

        /* --- MENÚ LATERAL (SIDEBAR) --- */
        aside {
            width: 280px;
            background-color: var(--bg-sidebar);
            position: fixed;
            top: 0;
            bottom: 0;
            left: 0;
            display: flex;
            flex-direction: column;
            padding: 30px 20px;
            border-right: 2px solid #222;
            z-index: 100;
        }

        .logo-container {
            text-align: center;
            margin-bottom: 10px;
        }

        .logo {
            font-size: 2rem;
            font-weight: 900;
            color: var(--primary-color);
            text-transform: uppercase;
            letter-spacing: 2px;
            text-shadow: 2px 2px 10px rgba(229, 9, 20, 0.4);
        }

        .slogan {
            font-size: 0.75rem;
            color: var(--text-muted);
            margin-bottom: 30px;
            font-style: italic;
        }

        .search-box {
            position: relative;
            margin-bottom: 25px;
        }

        .search-box input {
            width: 100%;
            padding: 10px 15px;
            background-color: #222;
            border: 1px solid #444;
            border-radius: 20px;
            color: white;
            font-size: 0.85rem;
        }

        .search-box input:focus {
            outline: none;
            border-color: var(--primary-color);
        }

        nav {
            display: flex;
            flex-direction: column;
            gap: 12px;
            flex-grow: 1;
        }

        nav a {
            display: flex;
            align-items: center;
            color: var(--text-main);
            text-decoration: none;
            padding: 12px 18px;
            border-radius: 8px;
            font-weight: 600;
            font-size: 0.95rem;
            transition: all 0.3s ease;
            border-left: 4px solid transparent;
        }

        nav a:hover {
            background-color: #1c1c1c;
            border-left-color: var(--primary-color);
            padding-left: 24px;
        }

        .sidebar-socials {
            display: flex;
            justify-content: space-around;
            margin-top: 20px;
            padding-top: 20px;
            border-top: 1px solid #222;
        }

        .social-btn {
            color: var(--text-muted);
            text-decoration: none;
            font-size: 0.8rem;
            font-weight: bold;
            padding: 6px 10px;
            border-radius: 5px;
            background: #1a1a1a;
            transition: 0.3s;
        }

        .social-btn.fb:hover { color: #1877F2; background: #fff; }
        .social-btn.tk:hover { color: #000000; background: #fff; }
        .social-btn.ig:hover { color: #E1306C; background: #fff; }

        /* --- CONTENEDOR PRINCIPAL --- */
        main {
            margin-left: 280px;
            flex-grow: 1;
            padding: 40px;
            position: relative;
        }

        /* --- SISTEMA DE PESTAÑAS AUTOMÁTICO --- */
        .content-section {
            display: none;
            animation: fadeIn 0.5s ease forwards;
        }

        #noticias:target, #nosotros:target, #contacto:target, #privacidad:target, #inicio:target {
            display: block;
        }

        :root:not(:has(:target)) #inicio {
            display: block;
        }

        @keyframes fadeIn {
            from { opacity: 0; transform: translateY(15px); }
            to { opacity: 1; transform: translateY(0); }
        }

        /* --- COMPONENTES REUTILIZABLES --- */
        h1 {
            font-size: 2.5rem;
            margin-bottom: 10px;
            border-bottom: 3px solid var(--primary-color);
            padding-bottom: 10px;
            display: inline-block;
        }

        .section-desc {
            color: var(--text-muted);
            margin-bottom: 30px;
            font-size: 1.1rem;
        }

        .cta-button {
            display: inline-block;
            background-color: var(--primary-color);
            color: white;
            padding: 12px 24px;
            border-radius: 5px;
            text-decoration: none;
            font-weight: bold;
            text-transform: uppercase;
            margin-top: 15px;
            transition: background 0.3s, transform 0.2s;
            border: none;
            cursor: pointer;
        }

        .cta-button:hover {
            background-color: #b80710;
            transform: scale(1.05);
        }

        .cta-secondary {
            background-color: #333;
            margin-left: 10px;
        }
        .cta-secondary:hover {
            background-color: #444;
        }

        .grid-container {
            display: grid;
            grid-template-columns: repeat(auto-fill, minmax(220px, 1fr));
            gap: 25px;
            margin-top: 20px;
        }

        .movie-card {
            background-color: var(--bg-card);
            border-radius: 8px;
            overflow: hidden;
            box-shadow: 0 4px 15px rgba(0,0,0,0.5);
            transition: transform 0.3s;
        }

        .movie-card:hover {
            transform: translateY(-8px);
        }

        .movie-img {
            width: 100%;
            height: 320px;
            object-fit: cover;
            background-color: #222;
            display: block;
        }

        .movie-info {
            padding: 15px;
        }

        .movie-title {
            font-size: 1rem;
            font-weight: bold;
            margin-bottom: 5px;
            height: 40px;
            display: flex;
            align-items: center;
        }

        .movie-price {
            color: var(--primary-color);
            font-weight: bold;
            font-size: 0.9rem;
            margin-bottom: 8px;
        }

        .movie-card p {
            font-size: 0.8rem;
            color: var(--text-muted);
            line-height: 1.4;
        }

        .featured-banner {
            background: linear-gradient(rgba(0,0,0,0.3), rgba(20,20,20,1)), url('https://image.tmdb.org/t/p/w1280/8Z8dptm6879v8v7M6OR6g84YMAw.jpg') center/cover;
            height: 380px;
            border-radius: 12px;
            display: flex;
            flex-direction: column;
            justify-content: flex-end;
            padding: 40px;
            margin-bottom: 40px;
            box-shadow: inset 0 0 100px rgba(0,0,0,0.8);
        }

        .video-box {
            margin: 40px 0;
            background: #000;
            padding: 20px;
            border-radius: 12px;
            text-align: center;
        }

        .video-placeholder {
            width: 100%;
            max-width: 800px;
            height: 450px;
            margin: 0 auto;
            background: #222;
            display: flex;
            align-items: center;
            justify-content: center;
            border: 2px dashed var(--primary-color);
            border-radius: 8px;
        }

        .tip-box {
            background-color: rgba(229, 9, 20, 0.1);
            border-left: 5px solid var(--primary-color);
            padding: 20px;
            border-radius: 4px;
            margin-top: 30px;
        }

        /* --- BLOG / NOTICIAS --- */
        .blog-container {
            display: flex;
            flex-direction: column;
            gap: 30px;
        }

        .blog-post {
            background-color: var(--bg-card);
            border-radius: 10px;
            padding: 25px;
            border-left: 4px solid var(--primary-color);
        }

        .blog-date {
            font-size: 0.8rem;
            color: var(--primary-color);
            font-weight: bold;
            margin-bottom: 10px;
        }

        /* --- NOSOTROS --- */
        .team-container {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(280px, 1fr));
            gap: 30px;
        }

        .team-card {
            background-color: var(--bg-sidebar);
            border: 1px solid #333;
            border-radius: 10px;
            padding: 25px;
            text-align: center;
        }

        .team-img {
            width: 120px;
            height: 120px;
            border-radius: 50%;
            object-fit: cover;
            margin-bottom: 15px;
            border: 3px solid var(--primary-color);
        }

        .team-role {
            color: var(--primary-color);
            font-weight: bold;
            font-size: 0.9rem;
            margin-bottom: 10px;
        }

        /* --- CONTACTO --- */
        .contacto-layout {
            display: grid;
            grid-template-columns: 1fr 1fr;
            gap: 40px;
        }

        .form-group {
            margin-bottom: 20px;
        }

        .form-group label {
            display: block;
            margin-bottom: 8px;
            font-size: 0.9rem;
            color: var(--text-muted);
        }

        .form-group input, .form-group textarea {
            width: 100%;
            padding: 12px;
            background-color: #222;
            border: 1px solid #444;
            border-radius: 6px;
            color: white;
        }

        .form-group input:focus, .form-group textarea:focus {
            outline: none;
            border-color: var(--primary-color);
        }

        .map-box {
            background: #222;
            height: 300px;
            border-radius: 10px;
            display: flex;
            align-items: center;
            justify-content: center;
            border: 1px solid #444;
            text-align: center;
            padding: 20px;
        }

        /* --- PRIVACIDAD --- */
        .legal-text {
            background-color: var(--bg-card);
            padding: 30px;
            border-radius: 8px;
            line-height: 1.6;
        }

        .legal-text h3 {
            margin: 20px 0 10px 0;
            color: var(--primary-color);
        }
    </style>
</head>
<body>

    <!-- MENÚ LATERAL -->
    <aside>
        <div class="logo-container">
            <div class="logo">Cuevaflix</div>
            <div class="slogan">Tu portal a las profundidades del cine</div>
        </div>

        <div class="search-box">
            <input type="text" placeholder="Buscar película o noticia...">
        </div>

        <nav>
            <a href="#inicio">🏠 Inicio / Cartelera</a>
            <a href="#noticias">📰 Noticias de Película</a>
            <a href="#nosotros">👥 Nosotros</a>
            <a href="#contacto">📞 Contacto</a>
            <a href="#privacidad">🔒 Privacidad</a>
        </nav>

        <div class="sidebar-socials">
            <a href="https://facebook.com" target="_blank" class="social-btn fb">Facebook</a>
            <a href="https://tiktok.com" target="_blank" class="social-btn tk">TikTok</a>
            <a href="https://instagram.com" target="_blank" class="social-btn ig">Instagram</a>
        </div>
    </aside>

    <!-- CONTENEDOR PRINCIPAL -->
    <main>

        <!-- ================= SECCIÓN INICIO ================= -->
        <section id="inicio" class="content-section">
            <div class="featured-banner">
                <h2>CINE REAL AL ALCANCE DE UN CLIC</h2>
                <p>Las mejores producciones de la historia de la animación y el cine comercial reunidas en un solo espacio estructurado.</p>
                <div>
                    <a href="#noticias" class="cta-button">Ver Noticias</a>
                    <a href="#contacto" class="cta-button cta-secondary">Suscribirse</a>
                </div>
            </div>

            <h1>Catálogo General de Películas Reales</h1>
            <p class="section-desc">Explora nuestra colección selecta de 20 películas icónicas reales con sus pósters de arte oficiales. Consulta descripciones detalladas y precios de renta digital.</p>

            <div class="grid-container">
                <!-- 1 -->
                <div class="movie-card">
                    <img class="movie-img" src="https://image.tmdb.org/t/p/w500/uXDfjJbdv4m6wS6H3jUBrvbg8TJ.jpg" alt="Toy Story">
                    <div class="movie-info">
                        <div class="movie-title">Toy Story</div>
                        <div class="movie-price">$59.00 MXN</div>
                        <p>Los juguetes de un niño cobran vida en secreto cuando él no está en la habitación, liderados por el intrepido vaquero Woody.</p>
                    </div>
                </div>
                <!-- 2 -->
                <div class="movie-card">
                    <img class="movie-img" src="https://image.tmdb.org/t/p/w500/iB64vpL3YAd69RI4kW56bI8I6Cj.jpg" alt="Shrek">
                    <div class="movie-info">
                        <div class="movie-title">Shrek</div>
                        <div class="movie-price">$49.00 MXN</div>
                        <p>Un ogro verde y solitario emprende una divertida misión junto a un burro parlanchín para rescatar a una hermosa princesa.</p>
                    </div>
                </div>
                <!-- 3 -->
                <div class="movie-card">
                    <img class="movie-img" src="https://image.tmdb.org/t/p/w500/kyeNo99E7xrYI8Sg46g6096v63B.jpg" alt="Avatar">
                    <div class="movie-info">
                        <div class="movie-title">Avatar</div>
                        <div class="movie-price">$79.00 MXN</div>
                        <p>Un ex-marine herido es transportado al asombroso y exótico mundo alienígena de Pandora, donde luchará por su supervivencia.</p>
                    </div>
                </div>
                <!-- 4 -->
                <div class="movie-card">
                    <img class="movie-img" src="https://image.tmdb.org/t/p/w500/iiZZ92gii6wI6wH2IDvOI2KA6vD.jpg" alt="Spider-Man">
                    <div class="movie-info">
                        <div class="movie-title">Spider-Man: Un Nuevo Universo</div>
                        <div class="movie-price">$69.00 MXN</div>
                        <p>El adolescente Miles Morales adquiere increíbles habilidades y descubre la existencia de un multiverso de héroes arácnidos.</p>
                    </div>
                </div>
                <!-- 5 -->
                <div class="movie-card">
                    <img class="movie-img" src="https://image.tmdb.org/t/p/w500/gEU2QniE6E77NI6wCU6MxlwZ7Zc.jpg" alt="Interstellar">
                    <div class="movie-info">
                        <div class="movie-title">Interstellar</div>
                        <div class="movie-price">$89.00 MXN</div>
                        <p>Un grupo de astronautas viaja a través de un agujero de gusano en el espacio profundo para salvar el destino de la humanidad.</p>
                    </div>
                </div>
                <!-- 6 -->
                <div class="movie-card">
                    <img class="movie-img" src="https://image.tmdb.org/t/p/w500/b1x0ptTXS9WvY786uwZp9f3jciC.jpg" alt="Jurassic Park">
                    <div class="movie-info">
                        <div class="movie-title">Jurassic Park</div>
                        <div class="movie-price">$59.00 MXN</div>
                        <p>Un parque científico recreativo con dinosaurios clonados genéticamente sufre un desastroso fallo en sus sistemas de contención.</p>
                    </div>
                </div>
                <!-- 7 -->
                <div class="movie-card">
                    <img class="movie-img" src="https://image.tmdb.org/t/p/w500/b0MxU77vOp1gvaA6Yp0ZJjHw0I7.jpg" alt="El Rey León">
                    <div class="movie-info">
                        <div class="movie-title">El Rey León</div>
                        <div class="movie-price">$49.00 MXN</div>
                        <p>Un joven cachorro de león huye de su reino tras una trágica traición familiar, aprendiendo el verdadero valor de su destino.</p>
                    </div>
                </div>
                <!-- 8 -->
                <div class="movie-card">
                    <img class="movie-img" src="https://image.tmdb.org/t/p/w500/wuMc0IejwN7vYXZbT80A61w362Y.jpg" alt="Harry Potter">
                    <div class="movie-info">
                        <div class="movie-title">Harry Potter y la Piedra Filosofal</div>
                        <div class="movie-price">$69.00 MXN</div>
                        <p>Un niño huérfano descubre en su cumpleaños que posee dotes mágicas y se inscribe en la legendaria academia de Hogwarts.</p>
                    </div>
                </div>
                <!-- 9 -->
                <div class="movie-card">
                    <img class="movie-img" src="https://image.tmdb.org/t/p/w500/8O76UInq61G27K77Xp6I7Yit4U1.jpg" alt="Piratas del Caribe">
                    <div class="movie-info">
                        <div class="movie-title">Piratas del Caribe</div>
                        <div class="movie-price">$59.00 MXN</div>
                        <p>El carismático y excéntrico capitán Jack Sparrow lucha por recuperar su navío de las manos de piratas víctimas de una maldición.</p>
                    </div>
                </div>
                <!-- 10 -->
                <div class="movie-card">
                    <img class="movie-img" src="https://image.tmdb.org/t/p/w500/f89U3wzN48oddHTw70H907m37S2.jpg" alt="Matrix">
                    <div class="movie-info">
                        <div class="movie-title">The Matrix</div>
                        <div class="movie-price">$65.00 MXN</div>
                        <p>Un hábil programador descubre que la realidad cotidiana en la que vive es una elaborada prisión mental digital.</p>
                    </div>
                </div>
                <!-- 11 -->
                <div class="movie-card">
                    <img class="movie-img" src="https://image.tmdb.org/t/p/w500/lR6N6mU6gYgY69w2Y8wH6W1C3pB.jpg" alt="IntensaMente">
                    <div class="movie-info">
                        <div class="movie-title">IntensaMente</div>
                        <div class="movie-price">$75.00 MXN</div>
                        <p>Las emociones básicas ubicadas en el cuartel mental de una pequeña niña compiten para guiarla en su proceso de adaptación.</p>
                    </div>
                </div>
                <!-- 12 -->
                <div class="movie-card">
                    <img class="movie-img" src="https://image.tmdb.org/t/p/w500/g8byw6N67SAr1Yw1e0Z0PZ8mG8L.jpg" alt="Coco">
                    <div class="movie-info">
                        <div class="movie-title">Coco</div>
                        <div class="movie-price">$69.00 MXN</div>
                        <p>Un niño mexicano impulsado por su gran amor a la música realiza un fantástico viaje místico al colorido Mundo de los Muertos.</p>
                    </div>
                </div>
                <!-- 13 -->
                <div class="movie-card">
                    <img class="movie-img" src="https://image.tmdb.org/t/p/w500/qJ2tWGBscGo17or6vC636w3tl3W.jpg" alt="Batman">
                    <div class="movie-info">
                        <div class="movie-title">Batman: El Caballero de la Noche</div>
                        <div class="movie-price">$85.00 MXN</div>
                        <p>El vigilante nocturno de Gotham une fuerzas con las autoridades para frenar la ola de caos provocada por el Guasón.</p>
                    </div>
                </div>
                <!-- 14 -->
                <div class="movie-card">
                    <img class="movie-img" src="https://image.tmdb.org/t/p/w500/7S09f87T4pQk1Hsc6G4mR2pQ42E.jpg" alt="Monsters Inc">
                    <div class="movie-info">
                        <div class="movie-title">Monsters, Inc.</div>
                        <div class="movie-price">$49.00 MXN</div>
                        <p>Dos eficientes monstruos encargados de recolectar gritos infantiles ven alterada su rutina cuando una niña cruza a su dimensión.</p>
                    </div>
                </div>
                <!-- 15 -->
                <div class="movie-card">
                    <img class="movie-img" src="https://image.tmdb.org/t/p/w500/9xjZS2rlVxm8SFX860GvkkjZ5QA.jpg" alt="Titanic">
                    <div class="movie-info">
                        <div class="movie-title">Titanic</div>
                        <div class="movie-price">$59.00 MXN</div>
                        <p>Una joven aristócrata y un humilde artista plástico viven un apasionado romance a bordo del transatlántico más famoso del mundo.</p>
                    </div>
                </div>
                <!-- 16 -->
                <div class="movie-card">
                    <img class="movie-img" src="https://image.tmdb.org/t/p/w500/RYMX2uS6M2wR6v7vU69E976C3p.jpg" alt="Avengers">
                    <div class="movie-info">
                        <div class="movie-title">Los Vengadores</div>
                        <div class="movie-price">$89.00 MXN</div>
                        <p>Los superhéroes más destacados de la Tierra forman una alianza histórica para repeler los ataques de un ejército invasor.</p>
                    </div>
                </div>
                <!-- 17 -->
                <div class="movie-card">
                    <img class="movie-img" src="https://image.tmdb.org/t/p/w500/kgwS7SuFo16g6ndO6O860o1IRrV.jpg" alt="Frozen">
                    <div class="movie-info">
                        <div class="movie-title">Frozen: Una Aventura Congelada</div>
                        <div class="movie-price">$55.00 MXN</div>
                        <p>Una intrépida joven emprende una épica travesía invernal junto a un recolector de hielo para salvar a su mágica hermana.</p>
                    </div>
                </div>
                <!-- 18 -->
                <div class="movie-card">
                    <img class="movie-img" src="https://image.tmdb.org/t/p/w500/khun7wZc5yWzZqTbe8lEPhfbaYy.jpg" alt="Mi Villano Favorito">
                    <div class="movie-info">
                        <div class="movie-title">Mi Villano Favorito</div>
                        <div class="movie-price">$49.00 MXN</div>
                        <p>Un genio criminal traza un plan maestro para robar la luna, viéndose obligado a hacerse cargo de tres adorables hermanas huérfanas.</p>
                    </div>
                </div>
                <!-- 19 -->
                <div class="movie-card">
                    <img class="movie-img" src="https://image.tmdb.org/t/p/w500/8Z8dptm6879v8v7M6OR6g84YMAw.jpg" alt="Ratatouille">
                    <div class="movie-info">
                        <div class="movie-title">Ratatouille</div>
                        <div class="movie-price">$59.00 MXN</div>
                        <p>Un roedor parisino dotado de un refinado paladar colabora secretamente con un inexperto joven en la cocina de un prestigioso restaurante.</p>
                    </div>
                </div>
                <!-- 20 -->
                <div class="movie-card">
                    <img class="movie-img" src="https://image.tmdb.org/t/p/w500/edv6w76l0C7ff0mZ2FXH17URm7Q.jpg" alt="Inception">
                    <div class="movie-info">
                        <div class="movie-title">El Origen (Inception)</div>
                        <div class="movie-price">$79.00 MXN</div>
                        <p>Un grupo de ladrones profesionales especializados en sustraer secretos corporativos se adentra en las capas profundas de los sueños.</p>
                    </div>
                </div>
            </div>

            <div class="video-box">
                <h3 style="margin-bottom: 15px;">Tráiler Oficial de la Plataforma Cuevaflix 2026</h3>
                <div class="video-placeholder">
                    <div style="color: var(--primary-color);">
                        <p style="font-size: 3rem;">▶</p>
                        <p style="color: white; margin-top: 10px;">[Video Demostrativo del Catálogo de Clásicos Animados]</p>
                    </div>
                </div>
            </div>

            <div class="tip-box">
                <h4>💡 Consejo Cuevaflix para cinéfilos:</h4>
                <p>Para disfrutar de clásicos como *Toy Story* o *Shrek* con la máxima fidelidad visual, te sugerimos ajustar la relación de aspecto de tu monitor a 16:9 nativo y activar el suavizado de bordes digitales.</p>
            </div>
        </section>

        <!-- ================= SECCIÓN NOTICIAS ================= -->
        <section id="noticias" class="content-section">
            <h1>Noticias de Película</h1>
            <p class="section-desc">Mantente al día con nuestro blog informativo ordenado cronológicamente desde los primeros anuncios históricos hasta hoy.</p>

            <div class="blog-container">
                <article class="blog-post">
                    <div class="blog-date">Publicado: 12 de Enero de 2026</div>
                    <h2>Lanzamiento oficial de la Cartelera Digital Cuevaflix</h2>
                    <p>Arranca formalmente nuestro espacio escolar de análisis. El equipo ha seleccionado cuidadosamente 20 títulos reales aclamados mundialmente por la crítica para enviar un portal dinámico, libre de fallos y adaptado para pantallas modernas.</p>
                </article>

                <article class="blog-post">
                    <div class="blog-date">Publicado: 15 de Marzo de 2026</div>
                    <h2>Aniversario y Legado Técnico de la Animación 3D</h2>
                    <p>Analizamos el impacto de *Toy Story* y cómo revolucionó la industria cinematográfica tras su estreno. Nuestros editores recopilan datos de rendimiento visual que demuestran el equilibrio perfecto entre el modelado y las historias memorables.</p>
                </article>

                <article class="blog-post">
                    <div class="blog-date">Publicado: 02 de Junio de 2026</div>
                    <h2>Tendencias de Cine Familiar en Quintana Roo</h2>
                    <p>Nuevas encuestas locales revelan que películas como *Shrek* e *IntensaMente* siguen encabezando los índices de reproducciones domésticas en el estado, consolidando la necesidad de plataformas informativas robustas y bien organizadas.</p>
                </article>
            </div>
        </section>

        <!-- ================= SECCIÓN NOSOTROS ================= -->
        <section id="nosotros" class="content-section">
            <h1>Nosotros</h1>
            <p class="section-desc">Conoce al equipo técnico fundador detrás del desarrollo estructural de Cuevaflix.</p>
            
            <p style="margin-bottom: 30px; line-height: 1.6;">Nuestra misión como equipo ha sido diseñar un catálogo intuitivo enfocado en obras reales del séptimo arte. A través de la correcta división del trabajo y una participación equitativa, construimos una interfaz cinemática oscura atractiva y libre de errores de código.</p>

            <div class="team-container">
                <div class="team-card">
                    <img class="team-img" src="https://images.unsplash.com/photo-1535713875002-d1d0cf377fde?q=80&w=150" alt="Emir">
                    <h3>Emir</h3>
                    <div class="team-role">Director de Logística e Infraestructura</div>
                    <p style="font-size: 0.9rem; color: var(--text-muted);">**Puesto:** Co-Fundador / Administrador Web</p>
                    <p style="margin-top: 10px; font-size: 0.85rem;">**Experiencia y Función:** Responsable de coordinar el orden lógico de las secciones y de la correcta ejecución de los hipervínculos del sitio.</p>
                </div>

                <div class="team-card">
                    <img class="team-img" src="https://images.unsplash.com/photo-1494790108377-be9c29b29330?q=80&w=150" alt="Leslie">
                    <h3>Leslie</h3>
                    <div class="team-role">Directora de Diseño Editorial y Estética</div>
                    <p style="font-size: 0.9rem; color: var(--text-muted);">**Puesto:** Co-Fundadora / Diseñadora de Layouts</p>
                    <p style="margin-top: 10px; font-size: 0.85rem;">**Experiencia y Función:** Encargada de la paleta tipográfica, el estilo visual oscuro y de garantizar un equilibrio armónico entre imágenes y texto.</p>
                </div>

                <div class="team-card">
                    <img class="team-img" src="https://images.unsplash.com/photo-1599566150163-29194dcaad36?q=80&w=150" alt="Abel">
                    <h3>Abel</h3>
                    <div class="team-role">Director de Contenidos Cinematográficos</div>
                    <p style="font-size: 0.9rem; color: var(--text-muted);">**Puesto:** Co-Fundador / Analista de Medios</p>
                    <p style="margin-top: 10px; font-size: 0.85rem;">**Experiencia y Función:** Encargado de la curaduría de sinopsis de las películas reales, la redacción del blog y la validación de avisos legales.</p>
                </div>
            </div>
        </section>

        <!-- ================= SECCIÓN CONTACTO ================= -->
        <section id="contacto" class="content-section">
            <h1>Contacto</h1>
            <p class="section-desc">Comunícate directamente con los integrantes del equipo o utiliza el formulario de recolección de datos seguro.</p>

            <div class="contacto-layout">
                <div>
                    <h3 style="margin-bottom: 20px;">Formulario de Atención al Usuario</h3>
                    <form action="#" method="POST" onsubmit="alert('¡Mensaje enviado correctamente a los administradores de Cuevaflix!'); return false;">
                        <div class="form-group">
                            <label for="nombre">Nombre Completo:</label>
                            <input type="text" id="nombre" placeholder="Ingresa tu nombre" required>
                        </div>
                        <div class="form-group">
                            <label for="email">Correo Electrónico:</label>
                            <input type="email" id="email" placeholder="correo@ejemplo.com" required>
                        </div>
                        <div class="form-group">
                            <label for="asunto">Asunto del Mensaje:</label>
                            <input type="text" id="asunto" placeholder="Ej. Sugerencia de película, Dudas..." required>
                        </div>
                        <div class="form-group">
                            <label for="mensaje">Mensaje:</label>
                            <textarea id="mensaje" rows="5" placeholder="Escribe aquí tus comentarios..." required></textarea>
                        </div>
                        <button type="submit" class="cta-button">Enviar Formulario</button>
                    </form>
                </div>

                <div>
                    <h3 style="margin-bottom: 20px;">Directorio de Contacto</h3>
                    <table style="width: 100%; border-collapse: collapse; margin-bottom: 30px; font-size: 0.9rem; text-align: left;">
                        <thead>
                            <tr style="border-bottom: 2px solid var(--primary-color);">
                                <th style="padding: 8px;">Integrante</th>
                                <th style="padding: 8px;">Correo</th>
                                <th style="padding: 8px;">Teléfono</th>
                            </tr>
                        </thead>
                        <tbody>
                            <tr style="border-bottom: 1px solid #222;">
                                <td style="padding: 8px;">Emir</td>
                                <td style="padding: 8px;">emir.admin@cuevaflix.com</td>
                                <td style="padding: 8px;">+52 997 123 4567</td>
                            </tr>
                            <tr style="border-bottom: 1px solid #222;">
                                <td style="padding: 8px;">Leslie</td>
                                <td style="padding: 8px;">leslie.design@cuevaflix.com</td>
                                <td style="padding: 8px;">+52 997 765 4321</td>
                            </tr>
                            <tr style="border-bottom: 1px solid #222;">
                                <td style="padding: 8px;">Abel</td>
                                <td style="padding: 8px;">abel.media@cuevaflix.com</td>
                                <td style="padding: 8px;">+52 997 987 6543</td>
                            </tr>
                        </tbody>
                    </table>

                    <h3 style="margin-bottom: 10px;">Ubicación de Oficinas Administrativas</h3>
                    <div class="map-box">
                        <div>
                            <p style="color: var(--primary-color); font-weight: bold; margin-bottom: 5px;">📍 Servidor Central Cuevaflix</p>
                            <p style="font-size: 0.85rem; color: var(--text-muted);">Colonia Centro, José María Morelos, Quintana Roo, México</p>
                            <div style="margin-top: 15px; width: 230px; height: 100px; background: #333; border-radius: 6px; display: flex; align-items: center; justify-content: center; margin: 15px auto 0 auto;">
                                <span style="font-size: 0.7rem; color:#888;">[Mapeo de Coordenadas GPS Habilitado]</span>
                            </div>
                        </div>
                    </div>
                </div>
            </div>
        </section>

        <!-- ================= SECCIÓN PRIVACIDAD ================= -->
        <section id="privacidad" class="content-section">
            <h1>Avisos de Políticas de Privacidad</h1>
            <p class="section-desc">Cumplimiento reglamentario legal y protección de derechos de autor de Cuevaflix.</p>

            <div class="legal-text">
                <h3>1. Tratamiento Responsable de Datos</h3>
                <p>Cuevaflix informa a la comunidad que la información recabada mediante los campos del formulario de contacto se utilizará de manera confidencial exclusivamente para fines de atención al usuario y retroalimentación escolar, garantizando los derechos ARCO conforme a la normativa vigente.</p>

                <h3>2. Respeto y Garantía de Derechos de Autor</h3>
                <p>Las marcas, posters promocionales, nombres de películas e información técnica correspondientes a obras reales expuestas en el catálogo (tales como *Toy Story* propiedad de Disney/Pixar, y *Shrek* propiedad de DreamWorks Pictures) pertenecen a sus respectivos creadores y titulares legales. Su uso en este sitio web es estrictamente de carácter educativo e ilustrativo académico sin fines de lucro.</p>

                <h3>3. Legislación Vigente</h3>
                <p>Este sitio opera bajo los lineamientos jurídicos aplicables de la Ley Federal de Protección de Datos Personales en Posesión de los Particulares y la Ley Federal del Derecho de Autor vigentes en los Estados Unidos Mexicanos.</p>
            </div>
        </section>

    </main>

</body>
</html>
