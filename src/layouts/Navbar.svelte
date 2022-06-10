<script>
    import { Link } from 'svelte-routing';
</script>

<nav>
    <div class="nav-wrapper">
        <Link to="/" class="brand-logo">Svelte Tutorial</Link>
        <ul id="nav-mobile" class="right hide-on-med-and-down">
            <li><Link to="/">Home</Link></li>
            <li><Link to="/page/about/">About</Link></li>
            <li><Link to="/page/contact/">Contact</Link></li>
        </ul>
    </div>
</nav>
