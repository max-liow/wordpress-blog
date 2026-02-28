### File Structure
.
└── wp-content/
    └── themes/
        └── your-theme/
            ├── css/
            │   └── prettify.css
            └── js/
                └── prettify.js


### Instruction
- 把以上的 CSS 和 JS file 复制在你的Themes 中，然后在your-theme/functions.php 加上这一段
```
function add_prettify_to_posts() {
    if ( is_single() || is_page() ) {
        wp_enqueue_script( 
            'prettify-js', 
            get_template_directory_uri() . '/js/prettify/prettify.js', 
            array(), 
            'r298', 
            true 
        );
        
        wp_enqueue_style( 
            'prettify-css', 
            get_template_directory_uri() . '/css/prettify.css', 
            array(), 
            'r298' 
        );
    }
}
add_action( 'wp_enqueue_scripts', 'add_prettify_to_posts' );


function auto_add_prettyprint_to_pre($content) {
    if ( is_single() || is_page() ) {
        $content = preg_replace_callback(
            '/<pre(?![^>]*class=)([^>]*)>(.*?)<\/pre>/is',
            function($matches) {
                return '<pre class="prettyprint" ' . $matches[1] . '>' . $matches[2] . '</pre>';
            },
            $content
        );
    }
    return $content;
}
add_filter('the_content', 'auto_add_prettyprint_to_pre', 20);

function init_prettify() {
    if ( is_single() || is_page() ) {
        echo '<script>!function(){var e=document,t=e.createElement("script");t.src="https://cdn.jsdelivr.net/gh/google/code-prettify@master/loader/run_prettify.js",e.body.appendChild(t)}();</script>';
    }
}
add_action('wp_footer', 'init_prettify');
```