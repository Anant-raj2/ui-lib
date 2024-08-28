<script>
    export let title = "Advanced Hover Card";
    export let content = "This is an advanced hover card with more detailed content and various sections. It is designed to provide a more interactive and visually appealing user experience.";
    export let imageUrl = "https://via.placeholder.com/150";
    export let socialLinks = {
        facebook: "#",
        twitter: "#",
        linkedin: "#",
    };
</script>

<style>
    .hover-card {
        position: relative;
        display: inline-block;
        margin: 20px;
        width: 300px;
        border-radius: 12px;
        overflow: hidden;
        box-shadow: 0 8px 16px rgba(0, 0, 0, 0.2);
        transition: transform 0.3s ease, box-shadow 0.3s ease;
    }

    .hover-card img {
        width: 100%;
        height: 200px;
        object-fit: cover;
        transition: transform 0.3s ease;
    }

    .hover-card:hover img {
        transform: scale(1.05);
    }

    .hover-card:hover {
        transform: translateY(-10px);
        box-shadow: 0 12px 24px rgba(0, 0, 0, 0.3);
    }

    .overlay {
        position: absolute;
        top: 0;
        left: 0;
        width: 100%;
        height: 100%;
        background-color: rgba(0, 0, 0, 0.5);
        opacity: 0;
        transition: opacity 0.3s ease;
        z-index: 1;
    }

    .hover-card:hover .overlay {
        opacity: 1;
    }

    .card-content {
        position: relative;
        padding: 20px;
        background-color: white;
        transition: opacity 0.3s ease, transform 0.3s ease;
        opacity: 0;
        transform: translateY(20px);
        z-index: 2;
    }

    .hover-card:hover .card-content {
        opacity: 1;
        transform: translateY(0);
    }

    .card-title {
        font-weight: bold;
        font-size: 20px;
        margin-bottom: 10px;
        color: #333;
    }

    .card-text {
        font-size: 14px;
        color: #555;
        max-height: 100px;
        overflow-y: auto;
        padding-right: 10px;
    }

    .social-icons {
        display: flex;
        justify-content: space-around;
        margin-top: 15px;
    }

    .social-icons a {
        text-decoration: none;
        color: #555;
        font-size: 24px;
        transition: color 0.3s ease;
    }

    .social-icons a:hover {
        color: #007BFF;
    }

    /* Scrollbar Styling */
    .card-text::-webkit-scrollbar {
        width: 6px;
    }

    .card-text::-webkit-scrollbar-track {
        background: #f1f1f1;
    }

    .card-text::-webkit-scrollbar-thumb {
        background: #888;
        border-radius: 4px;
    }

    .card-text::-webkit-scrollbar-thumb:hover {
        background: #555;
    }
</style>

<div class="hover-card">
    <div class="overlay"></div>
    <img src={imageUrl} alt={title} />
    <div class="card-content">
        <div class="card-title">{title}</div>
        <div class="card-text">{content}</div>
        <div class="social-icons">
            <a href={socialLinks.facebook} target="_blank" rel="noopener noreferrer">
                <i class="fab fa-facebook"></i>
            </a>
            <a href={socialLinks.twitter} target="_blank" rel="noopener noreferrer">
                <i class="fab fa-twitter"></i>
            </a>
            <a href={socialLinks.linkedin} target="_blank" rel="noopener noreferrer">
                <i class="fab fa-linkedin"></i>
            </a>
        </div>
    </div>
</div>

<!-- Add Font Awesome Icons -->
<script>
    import "@fortawesome/fontawesome-free/css/all.min.css";
</script>

