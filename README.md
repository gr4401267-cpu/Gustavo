<!DOCTYPE html>
<html lang="pt-BR">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
  <title>GX • Minecraft Content</title>
  <script src="https://cdn.tailwindcss.com"></script>
  <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.6.0/css/all.min.css">
  <style>
    body { font-family: 'Inter', system-ui, sans-serif; }
    .title-font { font-family: 'Press Start 2P', system-ui; }
    
    .post-card {
      transition: all 0.4s cubic-bezier(0.4, 0, 0.2, 1);
    }
    .post-card:hover {
      transform: translateY(-12px);
      box-shadow: 0 25px 50px -12px rgb(0 0 0 / 0.4);
    }
    .post-image { aspect-ratio: 16 / 13; object-fit: cover; }
    .tab-active { 
      border-bottom: 4px solid #22c55e; 
      color: white;
      font-weight: 600;
    }
  </style>
</head>
<body class="bg-zinc-950 text-white min-h-screen pb-20">

  <!-- HEADER -->
  <header class="bg-black/80 backdrop-blur-xl border-b border-green-500/30 sticky top-0 z-50">
    <div class="max-w-5xl mx-auto px-4 py-5 flex justify-between items-center">
      <div class="flex items-center gap-4">
        <div class="w-12 h-12 bg-gradient-to-br from-green-400 to-emerald-500 rounded-2xl flex items-center justify-center text-4xl shadow-lg">⛏️</div>
        <div>
          <h1 class="text-3xl font-bold title-font tracking-wider text-white">GX Studios</h1>
          <p class="text-emerald-400 text-sm -mt-1">    GX    </p>
        </div>
      </div>
      
      <div class="flex items-center gap-3">
        <input type="text" id="admin-code" placeholder="CÓDIGO ADMIN" 
               class="bg-zinc-900 border border-zinc-700 focus:border-emerald-500 px-5 py-3 rounded-2xl text-sm w-40 placeholder-zinc-500 focus:outline-none">
        <button onclick="handleCode()" 
                class="bg-emerald-500 hover:bg-emerald-400 transition-colors text-black font-semibold px-8 py-3 rounded-2xl">
          ENTRAR
        </button>
      </div>
    </div>

    <!-- TABS -->
    <div class="max-w-5xl mx-auto px-4 flex border-t border-zinc-800">
      <button onclick="switchTab(0)" id="tab-0" class="tab-active flex-1 py-5 text-center text-sm">ADDONS</button>
      <button onclick="switchTab(1)" id="tab-1" class="flex-1 py-5 text-center text-sm">TEXTURAS</button>
    </div>
  </header>

  <div class="max-w-5xl mx-auto px-4 pt-8">
    <div class="flex justify-between items-center mb-8">
      <h2 id="section-title" class="text-3xl font-bold flex items-center gap-3">
        <span class="text-emerald-400">📦</span> Addons
      </h2>
      <button id="new-post-btn" onclick="newPost()" 
              class="hidden items-center gap-3 bg-emerald-500 hover:bg-emerald-400 text-black font-semibold px-8 py-4 rounded-3xl transition-all">
        <i class="fas fa-plus"></i> NOVO CONTEÚDO
      </button>
    </div>

    <div id="posts-container" class="grid grid-cols-2 md:grid-cols-3 gap-6"></div>
  </div>

  <!-- Botão Flutuante -->
  <button id="float-new-btn" onclick="newPost()" 
          class="hidden fixed bottom-8 right-8 bg-emerald-500 text-black w-16 h-16 rounded-3xl flex items-center justify-center text-4xl shadow-2xl z-50 hover:scale-110 transition">
    +
  </button>

  <!-- Error Message -->
  <div id="error-message" class="hidden fixed bottom-8 left-1/2 -translate-x-1/2 bg-red-600/90 text-white px-10 py-4 rounded-2xl shadow-2xl text-sm font-medium backdrop-blur-md">
    Código incorreto
  </div>

  <!-- MODAL -->
  <div id="post-modal" class="hidden fixed inset-0 bg-black/90 flex items-end justify-center z-[100]">
    <div class="bg-zinc-900 w-full max-w-lg rounded-t-3xl max-h-[92vh] overflow-auto">
      <div class="p-8">
        <div class="flex justify-between mb-8">
          <h3 id="modal-title" class="text-2xl font-bold text-emerald-400">Novo Conteúdo</h3>
          <button onclick="closeModal()" class="text-4xl text-zinc-400 hover:text-white">✕</button>
        </div>

        <form id="post-form" class="space-y-6">
          <div>
            <label class="block text-sm mb-2 text-zinc-400">Categoria</label>
            <select id="category" class="w-full px-5 py-4 bg-zinc-800 border border-zinc-700 rounded-2xl text-white">
              <option value="addons">Addon</option>
              <option value="texturas">Textura</option>
            </select>
          </div>

          <div>
            <label class="block text-sm mb-2 text-zinc-400">Capa</label>
            <div onclick="document.getElementById('image-upload').click()" 
                 class="border-2 border-dashed border-zinc-700 rounded-3xl p-12 text-center cursor-pointer hover:border-emerald-500 transition">
              <input type="file" id="image-upload" accept="image/*" class="hidden" onchange="previewImage(event)">
              <div id="image-preview" class="hidden mb-4">
                <img id="preview-img" class="mx-auto rounded-2xl max-h-56 shadow-md">
              </div>
              <i class="fas fa-cloud-upload-alt text-5xl text-zinc-500 mb-3"></i>
              <p class="text-zinc-400">Clique ou toque para adicionar imagem</p>
            </div>
          </div>

          <div>
            <label class="block text-sm mb-2 text-zinc-400">Título</label>
            <input type="text" id="title" required class="w-full px-6 py-4 bg-zinc-800 border border-zinc-700 rounded-2xl focus:border-emerald-500">
          </div>

          <div>
            <label class="block text-sm mb-2 text-zinc-400">Descrição</label>
            <textarea id="description" rows="4" class="w-full px-6 py-4 bg-zinc-800 border border-zinc-700 rounded-3xl focus:border-emerald-500"></textarea>
          </div>

          <div>
            <label class="block text-sm mb-2 text-zinc-400">Link de Download</label>
            <input type="url" id="link" required class="w-full px-6 py-4 bg-zinc-800 border border-zinc-700 rounded-2xl focus:border-emerald-500">
          </div>

          <div class="flex gap-4 pt-6">
            <button type="button" onclick="closeModal()" class="flex-1 py-4 border border-zinc-700 rounded-2xl font-medium">Cancelar</button>
            <button type="submit" id="submit-btn" class="flex-1 py-4 bg-emerald-500 text-black font-bold rounded-2xl">PUBLICAR</button>
          </div>
        </form>
      </div>
    </div>
  </div>

  <script>
    // (O script continua o mesmo do anterior, só com pequenas melhorias visuais)
    let posts = JSON.parse(localStorage.getItem('moluscoContent')) || [];
    let isAdmin = false;
    let editingId = null;
    let currentTab = 0;

    const categories = ['addons', 'texturas', 'scripts'];
    const tabNames = [' Addons', 'Texturas', 'scripts'];

    function savePosts() { localStorage.setItem('moluscoContent', JSON.stringify(posts)); }

    function copyLink(link) {
      navigator.clipboard.writeText(link).then(() => alert("✅ Link copiado para a área de transferência"));
    }

    function deletePost(id) {
      if (!isAdmin || !confirm("Excluir este conteúdo?")) return;
      posts = posts.filter(p => p.id !== id);
      savePosts();
      renderPosts();
    }

    function editPost(id) {
      if (!isAdmin) return;
      const post = posts.find(p => p.id === id);
      if (!post) return;
      editingId = id;
      document.getElementById('modal-title').textContent = "Editar Conteúdo";
      document.getElementById('submit-btn').textContent = "SALVAR ALTERAÇÕES";
      document.getElementById('category').value = post.category;
      document.getElementById('title').value = post.title;
      document.getElementById('description').value = post.description;
      document.getElementById('link').value = post.link;
      document.getElementById('preview-img').src = post.image;
      document.getElementById('image-preview').classList.remove('hidden');
      document.getElementById('post-modal').classList.remove('hidden');
    }

    function newPost() {
      if (!isAdmin) return;
      editingId = null;
      document.getElementById('modal-title').textContent = "Novo Conteúdo";
      document.getElementById('submit-btn').textContent = "PUBLICAR";
      document.getElementById('post-form').reset();
      document.getElementById('image-preview').classList.add('hidden');
      document.getElementById('post-modal').classList.remove('hidden');
    }

    function closeModal() {
      document.getElementById('post-modal').classList.add('hidden');
    }

    function previewImage(e) {
      const reader = new FileReader();
      reader.onload = ev => {
        document.getElementById('preview-img').src = ev.target.result;
        document.getElementById('image-preview').classList.remove('hidden');
      };
      reader.readAsDataURL(e.target.files[0]);
    }

    function switchTab(tab) {
      currentTab = tab;
      document.querySelectorAll('[id^="tab-"]').forEach((el, i) => el.classList.toggle('tab-active', i === tab));
      document.getElementById('section-title').innerHTML = tabNames[tab];
      renderPosts();
    }

    function showError() {
      const error = document.getElementById('error-message');
      error.classList.remove('hidden');
      setTimeout(() => error.classList.add('hidden'), 2800);
    }

    function handleCode() {
      const code = document.getElementById('admin-code').value.trim().toUpperCase();
      
      if (code === "GX") {
        isAdmin = true;
        document.getElementById('new-post-btn').classList.remove('hidden');
        document.getElementById('float-new-btn').classList.remove('hidden');
        alert("✅ Modo Administrador ativado com sucesso!");
        renderPosts();
      } else if (code === "SAIR" && isAdmin) {
        isAdmin = false;
        document.getElementById('new-post-btn').classList.add('hidden');
        document.getElementById('float-new-btn').classList.add('hidden');
        alert("👋 Você saiu do modo administrador.");
        renderPosts();
      } else {
        showError();
      }
      
      document.getElementById('admin-code').value = '';
    }

    function renderPosts() {
      const container = document.getElementById('posts-container');
      container.innerHTML = '';

      const filtered = posts.filter(p => p.category === categories[currentTab]);

      if (filtered.length === 0) {
        container.innerHTML = `<div class="col-span-3 text-center py-24 text-zinc-400">Nenhum conteúdo publicado nesta categoria ainda.</div>`;
        return;
      }

      filtered.forEach(post => {
        const html = `
          <div class="post-card bg-zinc-900 rounded-3xl overflow-hidden border border-zinc-800">
            ${isAdmin ? `
            <div class="absolute top-4 right-4 flex gap-2 z-10">
              <button onclick="editPost(${post.id})" class="bg-blue-600 hover:bg-blue-500 w-9 h-9 rounded-2xl flex items-center justify-center text-lg">✏️</button>
              <button onclick="deletePost(${post.id})" class="bg-red-600 hover:bg-red-500 w-9 h-9 rounded-2xl flex items-center justify-center text-lg">✕</button>
            </div>` : ''}
            
            <img src="${post.image}" class="post-image w-full">
            
            <div class="p-5">
              <h3 class="font-semibold text-lg mb-2 line-clamp-2">${post.title}</h3>
              <p class="text-zinc-400 text-sm mb-6 line-clamp-4">${post.description}</p>
              <a href="${post.link}" target="_blank" class="block w-full text-center bg-emerald-500 hover:bg-emerald-400 text-black font-semibold py-4 rounded-2xl transition">
                BAIXAR AGORA
              </a>
            </div>
          </div>`;
        container.innerHTML += html;
      });
    }

    document.getElementById('post-form').addEventListener('submit', function(e) {
      e.preventDefault();
      const category = document.getElementById('category').value;
      const title = document.getElementById('title').value;
      const description = document.getElementById('description').value || "Conteúdo de qualidade para Minecraft";
      const link = document.getElementById('link').value;
      const image = document.getElementById('preview-img').src || `https://picsum.photos/800/650?random=${Date.now()}`;

      if (editingId) {
        const post = posts.find(p => p.id === editingId);
        if (post) Object.assign(post, {category, title, description, link, image: image.startsWith('data:') ? image : post.image});
      } else {
        posts.unshift({ id: Date.now(), category, title, description, link, image });
      }

      savePosts();
      renderPosts();
      closeModal();
      alert(editingId ? "✅ Alterações salvas com sucesso!" : "✅ Conteúdo publicado!");
    });

    window.onload = () => renderPosts();
  </script>
</body>
</html>
      
