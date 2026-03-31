// 1. DADOS DOS PORTFÓLIOS
// Este objeto armazena as informações de cada projeto, incluindo as URLs das imagens ilustrativas.
const portfolioData = {
    restaurante: {
        title: 'Banner Restaurante Premium',
        category: 'Restaurante',
        description: 'Design profissional de banner para restaurante com foco em qualidade e elegância.',
        images: [
            'https://files.manuscdn.com/user_upload_by_module/session_file/310519663499310108/klcZHDLqDDkAJTIG.png',
            'https://images.unsplash.com/photo-1414235077428-338989a2e8c0?w=800&q=80',
            'https://images.unsplash.com/photo-1517248135467-4c7edcad34c4?w=800&q=80'
        ]
    },
    // ... (outras categorias seguem o mesmo padrão )
};

// 2. FILTRO DE PORTFÓLIO
// Função que oculta ou exibe os itens do grid baseando-se na categoria selecionada.
function filterPortfolio(event, category) {
    const buttons = document.querySelectorAll('.filter-btn');
    buttons.forEach(btn => btn.classList.remove('active'));
    event.target.classList.add('active');

    const items = document.querySelectorAll('.item');
    items.forEach(item => {
        if (category === 'todos' || item.dataset.category === category) {
            item.style.display = 'block';
            setTimeout(() => { item.style.opacity = '1'; }, 10);
        } else {
            item.style.display = 'none';
        }
    });
}

// 3. CONTROLE DO MODAL E GALERIA
// Abre o modal e preenche dinamicamente com os dados do projeto clicado.
function openModal(element, projectType) {
    const modal = document.getElementById('portfolioModal');
    const data = portfolioData[projectType];
    if (!data) return;

    document.getElementById('modalTitle').textContent = data.title;
    document.getElementById('modalCategory').textContent = data.category;
    document.getElementById('modalDescription').textContent = data.description;
    document.getElementById('modalImage').src = data.images[0];

    const thumbnailsContainer = document.getElementById('modalThumbnails');
    thumbnailsContainer.innerHTML = '';

    data.images.forEach((img, index) => {
        const thumbnail = document.createElement('div');
        thumbnail.className = 'thumbnail' + (index === 0 ? ' active' : '');
        thumbnail.innerHTML = `<img src="${img}" alt="Imagem ${index + 1}">`;
        thumbnail.onclick = () => changeImage(img, index);
        thumbnailsContainer.appendChild(thumbnail);
    });

    modal.style.display = 'block';
}

// Troca a imagem principal do modal ao clicar em uma miniatura.
function changeImage(imageSrc, index) {
    document.getElementById('modalImage').src = imageSrc;
    document.querySelectorAll('.thumbnail').forEach(thumb => thumb.classList.remove('active'));
    document.querySelectorAll('.thumbnail')[index].classList.add('active');
}

// Funções para fechar o modal.
function closeModal() {
    document.getElementById('portfolioModal').style.display = 'none';
}

window.onclick = function(event) {
    const modal = document.getElementById('portfolioModal');
    if (event.target === modal) closeModal();
}

// 4. SCROLL SUAVE
// Adiciona um efeito de rolagem suave ao clicar nos links de navegação.
document.querySelectorAll('a[href^="#"]').forEach(anchor => {
    anchor.addEventListener('click', function(e) {
        e.preventDefault();
        const target = document.querySelector(this.getAttribute('href'));
        if (target) {
            target.scrollIntoView({ behavior: 'smooth', block: 'start' });
        }
    });
});
