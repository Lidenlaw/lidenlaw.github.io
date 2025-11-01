
<html lang="fr">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Films à voir au cinéma - Votes synchronisés</title>
    <style>
        * {
            box-sizing: border-box;
            margin: 0;
            padding: 0;
        }

        body {
            font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, Arial, sans-serif;
            background: #f5f5f5;
            color: #333;
            line-height: 1.6;
            padding: 20px;
        }

        .container {
            max-width: 800px;
            margin: 0 auto;
            background: white;
            padding: 30px;
            border-radius: 12px;
            box-shadow: 0 2px 10px rgba(0,0,0,0.1);
        }

        h1 {
            text-align: center;
            color: #2c3e50;
            margin-bottom: 10px;
            font-size: 2.5em;
        }

        .subtitle {
            text-align: center;
            color: #7f8c8d;
            margin-bottom: 30px;
            font-size: 1.1em;
        }

        .sync-status {
            position: fixed;
            top: 20px;
            right: 20px;
            padding: 8px 16px;
            background: #e8f5e9;
            color: #2e7d32;
            border-radius: 20px;
            font-size: 14px;
            display: flex;
            align-items: center;
            gap: 8px;
            z-index: 1000;
        }

        .sync-status.error {
            background: #ffebee;
            color: #c62828;
        }

        .sync-dot {
            width: 8px;
            height: 8px;
            background: #2e7d32;
            border-radius: 50%;
            animation: pulse 2s infinite;
        }

        .sync-status.error .sync-dot {
            background: #c62828;
            animation: none;
        }

        @keyframes pulse {
            0% { opacity: 1; }
            50% { opacity: 0.3; }
            100% { opacity: 1; }
        }

        .add-movie-section {
            background: #ecf0f1;
            padding: 20px;
            border-radius: 8px;
            margin-bottom: 30px;
        }

        .add-movie-form {
            display: flex;
            gap: 10px;
            flex-wrap: wrap;
        }

        input[type="text"] {
            flex: 1;
            min-width: 200px;
            padding: 12px 16px;
            border: 2px solid #ddd;
            border-radius: 6px;
            font-size: 16px;
            transition: border-color 0.3s;
        }

        input[type="text"]:focus {
            outline: none;
            border-color: #3498db;
        }

        .add-button {
            background: #3498db;
            color: white;
            border: none;
            padding: 12px 24px;
            border-radius: 6px;
            font-size: 16px;
            cursor: pointer;
            transition: background-color 0.3s;
            font-weight: 600;
        }

        .add-button:hover {
            background: #2980b9;
        }

        .add-button:disabled {
            background: #95a5a6;
            cursor: not-allowed;
        }

        .movies-list {
            display: flex;
            flex-direction: column;
            gap: 15px;
        }

        .movie-item {
            display: flex;
            align-items: center;
            justify-content: space-between;
            background: #f8f9fa;
            padding: 20px;
            border-radius: 8px;
            transition: transform 0.2s, box-shadow 0.2s;
        }

        .movie-item:hover {
            transform: translateY(-2px);
            box-shadow: 0 4px 12px rgba(0,0,0,0.1);
        }

        .movie-info {
            display: flex;
            align-items: center;
            gap: 15px;
            flex: 1;
        }

        .movie-rank {
            font-size: 24px;
            font-weight: bold;
            color: #7f8c8d;
            min-width: 40px;
            text-align: center;
        }

        .movie-title {
            font-size: 18px;
            font-weight: 500;
            color: #2c3e50;
        }

        .vote-section {
            display: flex;
            align-items: center;
            gap: 15px;
        }

        .vote-count {
            font-size: 20px;
            font-weight: 600;
            color: #27ae60;
            min-width: 50px;
            text-align: right;
        }

        .vote-button {
            background: #27ae60;
            color: white;
            border: none;
            padding: 10px 20px;
            border-radius: 50px;
            font-size: 16px;
            cursor: pointer;
            transition: all 0.3s;
            display: flex;
            align-items: center;
            gap: 5px;
            font-weight: 500;
        }

        .vote-button:hover {
            background: #229954;
            transform: scale(1.05);
        }

        .vote-button:active {
            transform: scale(0.95);
        }

        .vote-button:disabled {
            background: #95a5a6 !important;
            cursor: not-allowed !important;
            transform: none !important;
        }

        .vote-button:disabled:hover {
            background: #95a5a6 !important;
            transform: none !important;
        }

        .empty-state {
            text-align: center;
            padding: 60px 20px;
            color: #95a5a6;
        }

        .empty-state h3 {
            margin-bottom: 10px;
            font-size: 1.5em;
        }

        .loading {
            text-align: center;
            padding: 40px;
            color: #7f8c8d;
        }

        @media (max-width: 600px) {
            .container {
                padding: 20px;
            }

            h1 {
                font-size: 2em;
            }

            .movie-item {
                flex-direction: column;
                gap: 15px;
                text-align: center;
            }

            .movie-info {
                flex-direction: column;
            }

            .vote-section {
                width: 100%;
                justify-content: center;
            }

            .sync-status {
                position: static;
                margin-bottom: 20px;
                justify-content: center;
            }
        }

        @keyframes slideIn {
            from {
                opacity: 0;
                transform: translateY(-20px);
            }
            to {
                opacity: 1;
                transform: translateY(0);
            }
        }

        .movie-item.new {
            animation: slideIn 0.3s ease-out;
        }
    </style>
</head>
<body>
    <div class="sync-status" id="syncStatus">
        <div class="sync-dot"></div>
        <span>Synchronisé</span>
    </div>

    <div class="container">
        <h1>🎬 Films à voir au cinéma</h1>
        <p class="subtitle">Votez pour les films que vous aimeriez voir!</p>

        <div class="add-movie-section">
            <form class="add-movie-form" id="addMovieForm">
                <input 
                    type="text" 
                    id="movieInput" 
                    placeholder="Ajouter un nouveau film..." 
                    required
                />
                <button type="submit" class="add-button" id="addButton">+ Ajouter</button>
            </form>
        </div>

        <div id="loadingState" class="loading">Chargement des films</div>
        
        <div id="moviesList" class="movies-list" style="display: none;">
            <!-- Les films seront ajoutés ici dynamiquement -->
        </div>

        <div id="emptyState" class="empty-state" style="display: none;">
            <h3>Aucun film pour le moment</h3>
            <p>Soyez le premier à ajouter un film!</p>
        </div>
    </div>

    <script>
        // Configuration JSONBin
        const BIN_ID = '69063b0fae596e708f3cf205';
        const API_KEY = '$2a$10$bomA4SsvndnxG6mGd5nde.wTfQEckOa9/74DJIruAGkznwpFgKlJi';
        const API_URL = `https://api.jsonbin.io/v3/b/${BIN_ID}`;
        
        let movies = [];
        let isUpdating = false;
        let votedMovies = JSON.parse(localStorage.getItem('votedMovies')) || [];

        // Fonction pour sauvegarder les votes locaux
        function saveVotedMovies() {
            localStorage.setItem('votedMovies', JSON.stringify(votedMovies));
        }

        // Fonction pour vérifier si l'utilisateur a déjà voté
        function hasVoted(movieId) {
            return votedMovies.includes(movieId);
        }

        // Fonction pour afficher le statut de synchronisation
        function updateSyncStatus(status, message = '') {
            const syncStatus = document.getElementById('syncStatus');
            const statusText = syncStatus.querySelector('span');
            
            if (status === 'error') {
                syncStatus.classList.add('error');
                statusText.textContent = message || 'Erreur de connexion';
            } else {
                syncStatus.classList.remove('error');
                statusText.textContent = message || 'Synchronisé';
            }
        }

        // Fonction pour récupérer les films depuis JSONBin
        async function fetchMovies() {
            try {
                const response = await fetch(API_URL, {
                    headers: {
                        'X-Master-Key': API_KEY
                    }
                });
                
                if (!response.ok) throw new Error('Erreur de récupération');
                
                const data = await response.json();
                movies = data.record.movies || [];
                updateSyncStatus('success');
                renderMovies();
                
            } catch (error) {
                console.error('Erreur:', error);
                updateSyncStatus('error', 'Erreur de chargement');
                // En cas d'erreur, afficher les films locaux s'il y en a
                renderMovies();
            }
        }

        // Fonction pour sauvegarder les films sur JSONBin
        async function saveMovies() {
            if (isUpdating) return;
            isUpdating = true;
            
            try {
                const response = await fetch(API_URL, {
                    method: 'PUT',
                    headers: {
                        'Content-Type': 'application/json',
                        'X-Master-Key': API_KEY
                    },
                    body: JSON.stringify({ movies })
                });
                
                if (!response.ok) throw new Error('Erreur de sauvegarde');
                
                updateSyncStatus('success', 'Sauvegardé');
                setTimeout(() => updateSyncStatus('success'), 2000);
                
            } catch (error) {
                console.error('Erreur:', error);
                updateSyncStatus('error', 'Erreur de sauvegarde');
            } finally {
                isUpdating = false;
            }
        }

        // Fonction pour ajouter un film
        async function addMovie(title) {
            const movie = {
                id: Date.now() + Math.random(),
                title: title.trim(),
                votes: 0
            };
            
            movies.push(movie);
            renderMovies();
            await saveMovies();
        }

        // Fonction pour voter
        async function vote(movieId) {
            // Vérifier si déjà voté
            if (hasVoted(movieId)) {
                alert('Vous avez déjà voté pour ce film!');
                return;
            }
            
            const movie = movies.find(m => m.id === movieId);
            if (movie) {
                movie.votes++;
                votedMovies.push(movieId);
                saveVotedMovies();
                renderMovies();
                await saveMovies();
                
                // Animation du compteur
                const voteCount = document.querySelector(`[data-movie-id="${movieId}"] .vote-count`);
                voteCount.classList.add('vote-update');
                setTimeout(() => voteCount.classList.remove('vote-update'), 500);
            }
        }

        // Fonction pour afficher les films
        function renderMovies() {
            const moviesList = document.getElementById('moviesList');
            const emptyState = document.getElementById('emptyState');
            const loadingState = document.getElementById('loadingState');
            
            loadingState.style.display = 'none';
            
            if (movies.length === 0) {
                moviesList.style.display = 'none';
                emptyState.style.display = 'block';
                return;
            }
            
            moviesList.style.display = 'flex';
            emptyState.style.display = 'none';
            
            // Trier par nombre de votes (décroissant)
            const sortedMovies = [...movies].sort((a, b) => b.votes - a.votes);
            
            moviesList.innerHTML = sortedMovies.map((movie, index) => {
                const voted = hasVoted(movie.id);
                return `
                <div class="movie-item" data-movie-id="${movie.id}">
                    <div class="movie-info">
                        <div class="movie-rank">#${index + 1}</div>
                        <div class="movie-title">${escapeHtml(movie.title)}</div>
                    </div>
                    <div class="vote-section">
                        <div class="vote-count">${movie.votes} vote${movie.votes !== 1 ? 's' : ''}</div>
                        ${voted 
                            ? '<button class="vote-button" disabled style="background: #95a5a6; cursor: not-allowed;"><span>✓ Voté</span></button>'
                            : `<button class="vote-button" onclick="vote(${movie.id})"><span>+1</span></button>`
                        }
                    </div>
                </div>
                `;
            }).join('');
        }

        // Fonction pour échapper le HTML
        function escapeHtml(text) {
            const map = {
                '&': '&amp;',
                '<': '&lt;',
                '>': '&gt;',
                '"': '&quot;',
                "'": '&#039;'
            };
            return text.replace(/[&<>"']/g, m => map[m]);
        }

        // Gestionnaire du formulaire
        document.getElementById('addMovieForm').addEventListener('submit', async (e) => {
            e.preventDefault();
            const input = document.getElementById('movieInput');
            const button = document.getElementById('addButton');
            const title = input.value.trim();
            
            if (title) {
                // Vérifier si le film n'existe pas déjà
                const exists = movies.some(m => 
                    m.title.toLowerCase() === title.toLowerCase()
                );
                
                if (exists) {
                    alert('Ce film est déjà dans la liste!');
                } else {
                    button.disabled = true;
                    button.textContent = 'Ajout...';
                    
                    await addMovie(title);
                    
                    input.value = '';
                    input.focus();
                    button.disabled = false;
                    button.textContent = '+ Ajouter';
                }
            }
        });

        // Actualiser automatiquement toutes les 5 secondes
        setInterval(() => {
            if (!isUpdating) {
                fetchMovies();
            }
        }, 5000);

        // Charger les films au démarrage
        fetchMovies();
    </script>
</body>
</html>
