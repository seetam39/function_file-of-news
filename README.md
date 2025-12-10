<?php
/**
 * ColorMag functions related to adding files.
 *
 * @link    https://developer.wordpress.org/themes/basics/theme-functions/
 *
 * @package ColorMag
 *
 * @since   ColorMag 1.0.0
 */

// Exit if accessed directly.
defined( 'ABSPATH' ) || exit;


/**
 * Define constants.
 */
require get_template_directory() . '/inc/base/class-colormag-constants.php';

/**
 * Helpers functions.
 */
require get_template_directory() . '/inc/helper/utils.php';

/**
 * Calling in the admin area for the Welcome Page as well as for the new theme notice too.
 */
if ( is_admin() ) {
	require get_template_directory() . '/inc/admin/class-colormag-admin.php';
	require get_template_directory() . '/inc/admin/class-colormag-dashboard.php';
	require get_template_directory() . '/inc/admin/class-colormag-welcome-notice.php';
	require get_template_directory() . '/inc/admin/class-colormag-theme-review-notice.php';
}

require get_template_directory() . '/inc/admin/class-colormag-changelog-parser.php';


///** ColorMag setup file, hooked for `after_setup_theme`. */
require COLORMAG_INCLUDES_DIR . '/colormag-setup.php';

/**
 * Base.
 */
// Generate WordPress filter hook dynamically.
require COLORMAG_INCLUDES_DIR . '/base/class-colormag-dynamic-filter.php';

// Generate dynamic CSS from styling options.
require_once COLORMAG_INCLUDES_DIR . '/base/class-colormag-dynamic-css.php';
require_once COLORMAG_INCLUDES_DIR . '/base/class-colormag-dynamic-builder-css.php';

// Adds classes to appropriate places.
require_once COLORMAG_INCLUDES_DIR . '/base/class-colormag-dynamic-classes.php';

// Adds classes to appropriate places.
require COLORMAG_INCLUDES_DIR . '/base/class-colormag-css-classes.php';

/**
 * Core.
 */
// ColorMag setup file, hooked for `after_setup_theme`.
require COLORMAG_INCLUDES_DIR . '/core/class-colormag-after-setup-theme.php';

// Load scripts.
require_once COLORMAG_INCLUDES_DIR . '/core/class-colormag-enqueue-scripts.php';

// Header Media.
require_once COLORMAG_INCLUDES_DIR . '/core/custom-header.php';

/**
 * Customizer.
 */
require_once COLORMAG_CUSTOMIZER_DIR . '/class-colormag-customizer.php';

// Load customind.
require_once COLORMAG_CUSTOMIZER_DIR . '/customind/init.php';

//require __DIR__ . '/../customind/init.php';

/**
 * @var \Customind\Core\Customind
 */
global $customind;
$customind->set_css_var_prefix( 'colormag' );
$customind->set_i18n_data(
	[
		'domain' => 'colormag',
	]
);
add_action(
	'after_setup_theme',
	function () use ( $customind ) {
		$customind->set_section_i18n(
			[
				/* Translators: 1: Panel Title. */
				'customizing-action' => __( 'Customizing ▶ %s', 'colormag' ),
				'customizing'        => __( 'Customizing', 'colormag' ),
			]
		);
	}
);

/**
 * Deprecated.
 */
// Load deprecated functions.
require_once COLORMAG_INCLUDES_DIR . '/deprecated/deprecated-filters.php';
require_once COLORMAG_INCLUDES_DIR . '/deprecated/deprecated-functions.php';
require_once COLORMAG_INCLUDES_DIR . '/deprecated/deprecated-hooks.php';

/**
 * Helper.
 */
// Load utils & helper functions.
require_once COLORMAG_INCLUDES_DIR . '/helper/class-colormag-utils.php';

/**
 * Meta Boxes.
 */
// Meta boxes function and classes.
require_once COLORMAG_INCLUDES_DIR . '/meta-boxes/class-colormag-meta-boxes.php';
require_once COLORMAG_INCLUDES_DIR . '/meta-boxes/class-colormag-meta-box-page-settings.php';

/**
 * Migration
 */
// Load demo import migration scripts.
require_once COLORMAG_INCLUDES_DIR . '/migration/demo-import-migration.php';

// Load migration scripts.
require_once COLORMAG_INCLUDES_DIR . '/migration/class-colormag-migration.php';

/**
 * Widgets
 */
// Load Widgets and Widgetized Area.
require_once COLORMAG_WIDGETS_DIR . '/class-colormag-widgets.php';

/**
 * Templates.
 */
// Template functions files.
require COLORMAG_INCLUDES_DIR . '/template-tags.php';
require COLORMAG_INCLUDES_DIR . '/builder-template-tags.php';
require COLORMAG_INCLUDES_DIR . '/template-functions.php';

// Svg icon class.
require COLORMAG_INCLUDES_DIR . '/class-colormag-svg-icons.php';

//Template hooks.
require COLORMAG_PARENT_DIR . '/template-parts/hooks/hook-functions.php';
require COLORMAG_PARENT_DIR . '/template-parts/hooks/builder.php';

require COLORMAG_PARENT_DIR . '/template-parts/hooks/header/header.php';
require COLORMAG_PARENT_DIR . '/template-parts/hooks/header/header-main.php';
require COLORMAG_PARENT_DIR . '/template-parts/hooks/header/top-bar.php';

require COLORMAG_PARENT_DIR . '/template-parts/hooks/content/content.php';

require COLORMAG_PARENT_DIR . '/template-parts/hooks/footer/footer.php';

/** WP_Query functions files. */
require COLORMAG_INCLUDES_DIR . '/colormag-wp-query.php';

/** Breadcrumb class. */
require_once COLORMAG_INCLUDES_DIR . '/class-breadcrumb-trail.php';
require_once COLORMAG_INCLUDES_DIR . '/class-colormag-starter-content.php';

/** Load functions */
require_once COLORMAG_INCLUDES_DIR . '/ajax.php';

/** Add the JetPack plugin support */
if ( defined( 'JETPACK__VERSION' ) ) {
	require_once COLORMAG_INCLUDES_DIR . '/compatibility/jetpack/jetpack.php';
}

/** Add the WooCommerce plugin support */
if ( class_exists( 'WooCommerce' ) ) {
	require_once COLORMAG_INCLUDES_DIR . '/compatibility/woocommerce/woocommerce.php';
}

/** Add the Elementor compatibility file */
if ( defined( 'ELEMENTOR_VERSION' ) ) {
	require_once COLORMAG_ELEMENTOR_DIR . '/elementor.php';
	require_once COLORMAG_ELEMENTOR_DIR . '/elementor-functions.php';
}

function get_assets_url() {
	// Get correct URL and path to wp-content.
	$content_url = untrailingslashit( dirname( dirname( get_stylesheet_directory_uri() ) ) );
	$content_dir = wp_normalize_path( untrailingslashit( WP_CONTENT_DIR ) );

	$url = str_replace( $content_dir, $content_url, wp_normalize_path( __DIR__ ) );
	$url = set_url_scheme( $url );

	return $url;
}

/**
 * Set the content width in pixels, based on the theme's design and stylesheet.
 *
 * Priority 0 to make it available to lower priority callbacks.
 *
 * @global int $content_width
 */
function colormag_set_content_width() {

	// This variable is intended to be overruled from themes.
	// Open WPCS issue: {@link https://github.com/WordPress-Coding-Standards/WordPress-Coding-Standards/issues/1043}.
	// phpcs:ignore WordPress.NamingConventions.PrefixAllGlobals.NonPrefixedVariableFound
	$GLOBALS['content_width'] = apply_filters( 'colormag_set_content_width', 800 );
}

add_filter( 'themegrill_demo_importer_show_main_menu', '__return_false' );

add_filter( 'themegrill_demo_importer_routes', 'colormag_demo_importer_routes', 10, 1 );

function colormag_demo_importer_routes( $routes ) {
	// Remove the existing routes from the TDI
	unset( $routes['themes.php?page=demo-importer&demo=:slug'] );
	unset( $routes['themes.php?page=demo-importer&browse=:sort'] );
	unset( $routes['themes.php?page=demo-importer&search=:query'] );
	unset( $routes['themes.php?page=demo-importer'] );

	// Add the new routes
	$routes['themes.php?page=colormag&tab=starter-templates&demo=:slug']    = 'preview';
	$routes['themes.php?page=colormag&tab=starter-templates&browse=:sort']  = 'sort';
	$routes['themes.php?page=colormag&tab=starter-templates&search=:query'] = 'search';
	$routes['themes.php?page=colormag&tab=starter-templates']               = 'sort';

	return $routes;
}

add_filter( 'themegrill_demo_importer_baseURL', 'colormag_demo_importer_baseURL', 10, 1 );

function colormag_demo_importer_baseURL( $base_url ) {
	// Update the base URL in the demo importer.
	$base_url = 'themes.php?page=colormag&tab=starter-templates';

	return $base_url;
}

add_filter( 'themegrill_demo_importer_redirect_link', 'colormag_demo_importer_redirect_url' );

function colormag_demo_importer_redirect_url( $redirect_url ) {
	// Update the base URL in the demo importer.
	$redirect_url = admin_url( 'themes.php?page=colormag&tab=starter-templates&browse=all' );

	return $redirect_url;
}

add_action( 'wp_ajax_install_plugin', 'colormag_plugin_action_callback' );
add_action( 'wp_ajax_activate_plugin', 'colormag_plugin_action_callback' );

function colormag_plugin_action_callback() {
	if ( ! isset( $_POST['security'] ) || ! wp_verify_nonce( $_POST['security'], 'colormag_demo_import_nonce' ) ) {
		wp_send_json_error( array( 'message' => 'Security check failed.' ) );
	}
	if ( ! current_user_can( 'install_plugins' ) ) {
		wp_send_json_error( array( 'message' => 'You are not allowed to perform this action.' ) );
	}

	$plugin      = sanitize_text_field( $_POST['plugin'] );
	$plugin_slug = sanitize_text_field( $_POST['slug'] );

	if ( colormag_is_plugin_installed( $plugin ) ) {
		if ( is_plugin_active( $plugin ) ) {
			wp_send_json_success( array( 'message' => 'Plugin is already activated.' ) );
		} else {
			// Activate the plugin
			$result = activate_plugin( $plugin );

			if ( is_wp_error( $result ) ) {
				wp_send_json_error( array( 'message' => 'Error activating the plugin.' ) );
			} else {
				wp_send_json_success( array( 'message' => 'Plugin activated successfully!' ) );
			}
		}
	} else {
		// Install and activate the plugin
		include_once ABSPATH . 'wp-admin/includes/plugin-install.php';
		include_once ABSPATH . 'wp-admin/includes/class-wp-upgrader.php';
		$plugin_info = plugins_api( 'plugin_information', array( 'slug' => $plugin_slug ) );
		$upgrader    = new Plugin_Upgrader( new WP_Ajax_Upgrader_Skin() );
		$result      = $upgrader->install( $plugin_info->download_link );

		if ( is_wp_error( $result ) ) {
			wp_send_json_error( array( 'message' => 'Error installing the plugin.' ) );
		}

		$result = activate_plugin( $plugin );

		if ( is_wp_error( $result ) ) {
			wp_send_json_error( array( 'message' => 'Error activating the plugin.' ) );
		} else {
			wp_send_json_success( array( 'message' => 'Plugin installed and activated successfully!' ) );
		}
	}
}

function colormag_is_plugin_installed( $plugin_path ) {
	$plugins = get_plugins();
	return isset( $plugins[ $plugin_path ] );
}

add_action( 'after_setup_theme', 'colormag_set_content_width', 0 );

/**
 * $content_width global variable adjustment as per layout option.
 */
function colormag_content_width() {

	global $post;
	global $content_width;

	if ( $post ) {
		$layout_meta = get_post_meta( $post->ID, 'colormag_page_layout', true );
	}

	if ( is_home() ) {
		$queried_id  = get_option( 'page_for_posts' );
		$layout_meta = get_post_meta( $queried_id, 'colormag_page_layout', true );
	}

	if ( empty( $layout_meta ) || is_archive() || is_search() ) {
		$layout_meta = 'default_layout';
	}

	$colormag_default_sidebar_layout = get_theme_mod( 'colormag_default_sidebar_layout', 'right_sidebar' );
	$colormag_page_sidebar_layout    = get_theme_mod( 'colormag_page_sidebar_layout', 'right_sidebar' );
	$colormag_default_post_layout    = get_theme_mod( 'colormag_post_sidebar_layout', 'right_sidebar' );

	if ( 'default_layout' === $layout_meta ) {
		if ( is_page() ) {
			if ( 'no_sidebar_full_width' === $colormag_page_sidebar_layout ) {
				$content_width = 1140; /* pixels */
			}
		} elseif ( is_single() ) {
			if ( 'no_sidebar_full_width' === $colormag_default_post_layout ) {
				$content_width = 1140; /* pixels */
			}
		} elseif ( 'no_sidebar_full_width' === $colormag_default_sidebar_layout ) {
				$content_width = 1140; /* pixels */
		}
	} elseif ( 'no_sidebar_full_width' === $layout_meta ) {
			$content_width = 1140; /* pixels */
	}
}

add_action( 'template_redirect', 'colormag_content_width' );

/**
 * Detect plugin. For use on Front End only.
 */
require_once ABSPATH . 'wp-admin/includes/plugin.php';

function colormag_maybe_enable_builder() {

	if ( get_option( 'colormag_builder_migration' ) || get_option( 'colormag_maybe_enable_builder' ) ) {
		return true;
	}

	if ( get_option( 'colormag_free_major_update_customizer_migration_v1' ) || get_option( 'colormag_top_bar_options_migrate' ) || get_option( 'colormag_breadcrumb_options_migrate' ) || get_option( 'colormag_social_icons_control_migrate' ) ) {
		return false;
	}

	update_option( 'colormag_maybe_enable_builder', true );

	return true;
}

function colormag_fresh_install() {

	if ( get_option( 'colormag_free_major_update_customizer_migration_v1' ) || get_option( 'colormag_top_bar_options_migrate' ) || get_option( 'colormag_breadcrumb_options_migrate' ) || get_option( 'colormag_social_icons_control_migrate' ) ) {
		return false;
	}

	return true;
}

/**
 * Binds JS handlers to make Theme Customizer preview reload changes asynchronously.
 *
 * @since ColorMag 3.0.0
 */
function cm_customize_preview_js() {

	if ( colormag_maybe_enable_builder() ) {
		set_theme_mod( 'colormag_enable_builder', true );
		update_option( 'colormag_builder_migration', true );
	}

	$suffix = ( defined( 'SCRIPT_DEBUG' ) && SCRIPT_DEBUG ) ? '' : '.min';

	wp_enqueue_script(
		'colormag-customizer-pre',
		get_assets_url() . '/inc/customizer/assets/js/cm-customize-preview.js',
		array(
			'customize-preview',
		),
		COLORMAG_THEME_VERSION,
		true
	);
}
add_action( 'customize_preview_init', 'cm_customize_preview_js' );

add_action(
	'customize_controls_enqueue_scripts',
	function () {
		add_filter(
			'customind_setting_data',
			function ( $data ) {
				$data['upgrade_notice']      = true;
				$data['upgrade_notice_text'] = __( 'Upgrade to ColorMag Pro for more features and customization options.', 'colormag' );
				$data['upgrade_notice_link'] = 'https://themegrill.com/pricing/?utm_medium=customizer-upsell&utm_source=colormag-theme&utm_campaign=upsell-button&utm_content=more-feature-in-pro';

				return $data;
			}
		);
	},
	11
);


// Add Google Fonts to block editor typography options
add_filter(
	'wp_theme_json_data_theme',
	function ( $json ) {

		if ( ! class_exists( 'WP_Theme_JSON_Data' ) || ! ( $json instanceof WP_Theme_JSON_Data ) ) {
			return $json;
		}
		$google_fonts = [
			[
				'name'       => 'DM Sans',
				'slug'       => 'dm-sans',
				'fontFamily' => 'DM Sans, sans-serif',
				'fontFace'   => [
					[
						'src'        => [
							'https://fonts.gstatic.com/s/dmsans/v15/rP2Hp2ywxg089UriCZOIHTWEBlw.woff2', // 400
							'https://fonts.gstatic.com/s/dmsans/v15/rP2Cp2ywxg089UriAWCrOBw.woff2', // 700
							'https://fonts.gstatic.com/s/dmsans/v15/rP2Hp2ywxg089UriCZOIHTWEBlw.woff2', // variable
						],
						'fontWeight' => '100 900',
						'fontStyle'  => 'normal',
						'fontFamily' => 'DM Sans',
					],
				],
			],
			[
				'name'       => 'Public Sans',
				'slug'       => 'public-sans',
				'fontFamily' => 'Public Sans, sans-serif',
				'fontFace'   => [
					[
						'src'        => [
							'https://fonts.gstatic.com/s/publicsans/v15/ijwOs5juQtsyLLR5jN4cxBEoRDf44uE.woff2', // 400
							'https://fonts.gstatic.com/s/publicsans/v15/ijwPs5juQtsyLLR5jN4cxBEoRDf44uE.woff2', // 700
							'https://fonts.gstatic.com/s/publicsans/v15/ijwOs5juQtsyLLR5jN4cxBEoRDf44uE.woff2', // variable
						],
						'fontWeight' => '100 900',
						'fontStyle'  => 'normal',
						'fontFamily' => 'Public Sans',
					],
				],
			],
			[
				'name'       => 'Roboto',
				'slug'       => 'roboto',
				'fontFamily' => 'Roboto, sans-serif',
				'fontFace'   => [
					[
						'src'        => [
							'https://fonts.gstatic.com/s/roboto/v30/KFOmCnqEu92Fr1Mu4mxM.woff2', // 400
							'https://fonts.gstatic.com/s/roboto/v30/KFOlCnqEu92Fr1MmWUlfBBc9.woff2', // 700
							'https://fonts.gstatic.com/s/roboto/v30/KFOjCnqEu92Fr1Mu51TjASc6CsE.woff2', // variable
						],
						'fontWeight' => '100 900',
						'fontStyle'  => 'normal',
						'fontFamily' => 'Roboto',
					],
				],
			],
			[
				'name'       => 'Segoe UI',
				'slug'       => 'segoe-ui',
				'fontFamily' => 'Segoe UI, Arial, sans-serif',
				'fontFace'   => [],
			],
		];

		$existing = $json->get_data()['settings']['typography']['fontFamilies']['theme'] ?? [];
		$json->update_with(
			[
				'version'  => 3,
				'settings' => [
					'typography' => [
						'fontFamilies' => [
							...$google_fonts,
							...$existing,
						],
					],
				],
			]
		);
		return $json;
	},
	10
);

// Enqueue Google Fonts in the block editor (all weights and variable fonts)
add_action(
	'enqueue_block_editor_assets',
	function () {
		wp_enqueue_style(
			'colormag-google-fonts',
			'https://fonts.googleapis.com/css2?family=DM+Sans:wght@100..900&family=Public+Sans:wght@100..900&family=Roboto:wght@100..900&display=swap',
			[],
			null
		);
	}
);

function colormag_typography_should_migrate() {
	// Default values for comparison
	$default_typography_presets   = '';
	$default_base_typography_body = array(
		'font-family'    => 'inherit',
		'font-weight'    => 'regular',
		'subsets'        => array( 'latin' ),
		'font-size'      => array(
			'desktop' => array(
				'size' => '15',
				'unit' => 'px',
			),
			'tablet'  => array(
				'size' => '',
				'unit' => 'px',
			),
			'mobile'  => array(
				'size' => '',
				'unit' => 'px',
			),
		),
		'line-height'    => array(
			'desktop' => array(
				'size' => '1.6',
				'unit' => '-',
			),
			'tablet'  => array(
				'size' => '',
				'unit' => '-',
			),
			'mobile'  => array(
				'size' => '',
				'unit' => '-',
			),
		),
		'letter-spacing' => array(
			'desktop' => array(
				'size' => '',
				'unit' => 'px',
			),
			'tablet'  => array(
				'size' => '',
				'unit' => 'px',
			),
			'mobile'  => array(
				'size' => '',
				'unit' => 'px',
			),
		),
	);

	$default_base_heading_typography = array(
		'font-family'    => 'inherit',
		'font-weight'    => 'regular',
		'subsets'        => array( 'latin' ),
		'line-height'    => array(
			'desktop' => array(
				'size' => '1.2',
				'unit' => '-',
			),
			'tablet'  => array(
				'size' => '',
				'unit' => '',
			),
			'mobile'  => array(
				'size' => '',
				'unit' => '',
			),
		),
		'letter-spacing' => array(
			'desktop' => array(
				'size' => '',
				'unit' => 'px',
			),
			'tablet'  => array(
				'size' => '',
				'unit' => 'px',
			),
			'mobile'  => array(
				'size' => '',
				'unit' => 'px',
			),
		),
		'font-style'     => 'normal',
		'text-transform' => 'none',
	);

	// Get current values
	$current_typography_presets      = get_theme_mod( 'colormag_typography_presets', $default_typography_presets );
	$current_base_typography_body    = get_theme_mod( 'colormag_base_typography', $default_base_typography_body );
	$current_base_heading_typography = get_theme_mod( 'colormag_headings_typography', $default_base_heading_typography );

	// Check if current values are different from default values
	$should_migrate = false;

	// Check typography presets
	if ( $current_typography_presets !== $default_typography_presets ) {
		$should_migrate = true;
	}

	// Check base typography body
	if ( $current_base_typography_body !== $default_base_typography_body ) {
		$should_migrate = true;
	}

	// Check base heading typography
	if ( $current_base_heading_typography !== $default_base_heading_typography ) {
		$should_migrate = true;
	}

	return $should_migrate;
}

function add_custom_social_buttons_small($content) {
    if (is_single()) {

        $social_html = '
        <style>
        .custom-social-wrap {
            display:flex;
            align-items:center;
            justify-content:space-between;
            margin:20px 0;
            flex-wrap:nowrap !important; /* Force single line */
            overflow-x:auto; /* Prevent overflow */
            gap:10px;
            padding-bottom:5px;
        }

        .social-icons {
            display:flex;
            gap:8px;
            flex-shrink:0;
        }

        .social-icons a {
            width:32px;
            height:32px;
            border-radius:50%;
            border:1.5px solid #000;
            display:flex;
            align-items:center;
            justify-content:center;
            text-decoration:none;
            color:#000;
            font-size:14px;
            transition:0.3s;
            flex-shrink:0;
        }

        .social-icons a:hover {
            background:#000;
            color:#fff;
        }

        .join-btn {
            display:flex;
            align-items:center;
            gap:8px;
            font-size:15px;
            font-weight:600;
            padding:8px 12px;
            background:#f4f4f4;
            border-radius:10px;
            text-decoration:none;
            color:#000;
            white-space:nowrap;
            flex-shrink:0;
        }

        .join-btn .wapp {
            width:32px;
            height:32px;
            border-radius:50%;
            background:#25d366;
            color:#fff;
            display:flex;
            align-items:center;
            justify-content:center;
            font-size:16px;
            flex-shrink:0;
        }

        /* ---------- RESPONSIVE SHRINKING ---------- */

        @media (max-width:768px) {
            .social-icons a {
                width:28px;
                height:28px;
                font-size:12px;
            }
            .join-btn {
                font-size:13px;
                padding:6px 10px;
                gap:6px;
            }
            .join-btn .wapp {
                width:26px;
                height:26px;
                font-size:13px;
            }
        }

        @media (max-width:480px) {
            .social-icons a {
                width:26px;
                height:26px;
                font-size:11px;
            }
            .join-btn {
                font-size:12px;
                padding:5px 8px;
                gap:5px;
            }
            .join-btn .wapp {
                width:24px;
                height:24px;
                font-size:12px;
            }
        }
        </style>

        <div class="custom-social-wrap">
    <div class="social-icons">
        <a href="https://www.facebook.com/VandeBharat24News"><i class="fab fa-facebook-f"></i></a>
        <a href="https://x.com/vandebharat24"><i class="fab fa-x-twitter"></i></a>
        <a href="https://www.instagram.com/vandebharat24news/"><i class="fab fa-instagram"></i></a>               
        <a href="https://www.youtube.com/@vandebharat24"><i class="fab fa-youtube"></i></a>
    </div>

    <a href="https://api.whatsapp.com/send?phone=917973997768&text=Hi%20Vande%20Bharat%2024" class="join-btn">
        Join our community
        <div class="wapp"><i class="fab fa-whatsapp"></i></div>
    </a>
</div>

        ';

        return $social_html . $content;
    }

    return $content;
}
add_filter("the_content", "add_custom_social_buttons_small");


// Sports page--------------------------------------------------------------------------------
function sports_widget_shortcode() {
    ob_start();

    /* -------------------------------
       QUERY: FEATURED (LEFT) POST
    -------------------------------- */
    $featured = new WP_Query([
        'category_name' => 'sports',
        'posts_per_page' => 1
    ]);
    ?>

<style>
/* MOBILE STYLE FIXES */
/* MOBILE FIX: Make layout vertical like original ColorMag */
@media (max-width: 768px) {

    /* parent layout should stack */
    .cm-featured-posts {
        display: block !important;
    }

    /* big post full width */
    .cm-first-post {
        width: 100% !important;
        margin-bottom: 20px !important;
    }

    /* right-side small posts BELOW big post */
    .cm-posts {
        width: 100% !important;
        margin-top: 10px !important;
        display: block !important;
    }

    /* small posts full width */
    .cm-posts .cm-post {
        display: flex !important;
        width: 100% !important;
        margin-bottom: 12px !important;
        border-radius: 10px !important;
        padding: 10px !important;
    }

    /* small image shrinking */
    .cm-posts .cm-post img {
        width: 100px !important;
        height: 70px !important;
        object-fit: cover !important;
        border-radius: 8px !important;
    }

    /* category badge smaller */
    .cm-post-categories a {
        padding: 3px 8px !important;
        font-size: 10px !important;
    }

    /* Live score box spacing */
    .live-score-box {
        margin-top: 20px !important;
        padding: 15px !important;
        border-radius: 12px !important;
    }

    .live-score-box iframe {
        height: 220px !important;
    }
}

</style>

<section id="sports-section" class="widget cm-featured-posts cm-featured-posts--style-1">

    <h3 class="cm-widget-title"><span>Sports</span></h3>

    <!-- LEFT FEATURED POST -->
    <div class="cm-first-post">
        <?php while($featured->have_posts()) : $featured->the_post(); ?>
        <div class="cm-post">

            <!-- Thumbnail -->
            <a href="<?php the_permalink(); ?>">
                <?php the_post_thumbnail('colormag-featured-post-medium'); ?>
            </a>

            <div class="cm-post-content">

                <!-- CATEGORY LABELS -->
                <div class="cm-entry-header-meta">
                    <div class="cm-post-categories">
                        <?php
                        $categories = get_the_category();
                        foreach ($categories as $cat) {
                            echo '<a href="' . get_category_link($cat->term_id) . '" 
                                style="
                                    background:#000;
                                    color:#fff;
                                    padding:5px 10px;
                                    border-radius:4px;
                                    font-size:10px;
                                    font-weight:500;
                                    text-transform:uppercase;
                                    margin-right:6px;
                                    display:inline-block;
                                ">'. esc_html($cat->name) .'</a>';
                        }
                        ?>
                    </div>
                </div>

                <!-- TITLE -->
                <h3 class="cm-entry-title">
                    <a href="<?php the_permalink(); ?>">
                        <?php the_title(); ?>
                    </a>
                </h3>

                <!-- META -->
                <div class="cm-below-entry-meta">
                    <span class="cm-post-date">
                        <time><?php echo get_the_date(); ?></time>
                    </span>

                    <span class="cm-author">
                        <a><?php the_author(); ?></a>
                    </span>
                </div>

                <!-- EXCERPT -->
                <div class="cm-entry-summary">
                    <p><?php echo wp_trim_words(get_the_excerpt(), 20); ?></p>
                </div>

            </div>
        </div>
        <?php endwhile; wp_reset_postdata(); ?>
    </div>



    <!-- RIGHT-SIDE POSTS -->
    <div class="cm-posts">

        <?php
        $right_posts = new WP_Query([
            'category_name' => 'sports',
            'posts_per_page' => 3,
            'offset' => 1
        ]);
        ?>

        <?php while($right_posts->have_posts()) : $right_posts->the_post(); ?>
        <div class="cm-post">

            <!-- Small Image -->
            <a href="<?php the_permalink(); ?>">
                <?php the_post_thumbnail('colormag-featured-post-small'); ?>
            </a>

            <div class="cm-post-content">

                <!-- CATEGORY BADGES -->
                <div class="cm-entry-header-meta">
                    <div class="cm-post-categories">
                        <?php
                        $categories = get_the_category();
                        foreach ($categories as $cat) {
                            echo '<a href="' . get_category_link($cat->term_id) . '" 
                                style="
                                    background:#000;
        color:#fff;
        padding:1px 4px;
        border-radius:3px;
        font-size:8px;
        font-weight:400;
        text-transform:uppercase;
        margin-right:5px;
        display:inline-block;
                                ">'. esc_html($cat->name) .'</a>';
                        }
                        ?>
                    </div>
                </div>

                <!-- Title -->
                <h3 class="cm-entry-title">
                    <a href="<?php the_permalink(); ?>">
                        <?php the_title(); ?>
                    </a>
                </h3>

                <!-- Meta -->
                <div class="cm-below-entry-meta">
                    <span class="cm-post-date">
                        <time><?php echo get_the_date(); ?></time>
                    </span>

                    <span class="cm-author">
                        <a><?php the_author(); ?></a>
                    </span>
                </div>

            </div>
        </div>
        <?php endwhile; wp_reset_postdata(); ?>


               <!-- ⭐ LIVE CRICKET SCORE BOX -->
<!--   <div style="max-width:100%;">
<style>
.score-widget-box {
  border-radius:10px;
  background:#ffffff;
  font-family: Arial, sans-serif;
  padding:12px;
}

/* tabs */
.score-tabs { display:flex; background:#f6f6f6; border-radius:8px; overflow:hidden; }
.score-tabs div { flex:1; text-align:center; padding:8px 6px; cursor:pointer; font-weight:700; }
.score-tabs .active { background:#bc0a48; color:#fff; }

/* content */
.score-row { padding:12px 6px; }

/* big card */
.cric-card { background:#fafafa; border-radius:8px; padding:12px; text-align:center; box-shadow:0 1px 0 rgba(0,0,0,0.04); }
.cric-series { font-size:14px; color:#666; margin-bottom:8px; }
.cric-teams { display:flex; justify-content:space-between; align-items:center; margin:8px 0; }
.cric-teams .team { width:40%; text-align:center; font-weight:700; font-size:16px; }
.cric-teams .vs { width:20%; text-align:center; font-weight:700; color:#333; }

/* score lines */
.cric-score-line { display:flex; justify-content:space-between; margin-top:6px; }
.cric-score-line .sc { width:40%; text-align:center; font-size:20px; font-weight:800; }
.cric-overs { font-size:14px; color:#555; margin-top:6px; }
.cric-status { margin-top:10px; color:#444; font-weight:600; }

/* meta bar */
.cric-meta { margin-top:10px; font-size:13px; color:#0a8f0d; display:flex; justify-content:space-between; }

.small-note { color:#777; font-size:13px; margin-top:10px; }
</style>

<div class="score-widget-box" id="scoreWidget">
  <div class="score-tabs">
    <div id="tabFixture">Fixture</div>
    <div id="tabLive" class="active">Live</div>
    <div id="tabResult">Result</div>
  </div>

  <div id="scoreContent" class="score-row">Loading...</div>
</div>

<script>
(function(){

  const URLS = {
    live: "https://ballbyball.in/api/live",
    fixture: "https://ballbyball.in/api/upcoming",
    result: "https://ballbyball.in/api/recent"
  };

  async function fetchData(type) {
    const res = await fetch(URLS[type]);
    return res.json();
  }

  function renderMatch(match) {
    const t1 = match.team1 || "Team A";
    const t2 = match.team2 || "Team B";
    const series = match.series || "";
    const status = match.status || "";

    const score1 = match.score1 || "";
    const score2 = match.score2 || "";
    const overs1 = match.overs1 ? `(${match.overs1})` : "";
    const overs2 = match.overs2 ? `(${match.overs2})` : "";

    return `
      <div class="cric-card">
        <div class="cric-series">${series}</div>

        <div class="cric-teams">
          <div class="team">${t1}</div>
          <div class="vs">VS</div>
          <div class="team">${t2}</div>
        </div>

        <div class="cric-score-line">
          <div class="sc">${score1}</div>
          <div class="sc">${score2}</div>
        </div>

        <div class="cric-overs">
          <div style="float:left">${overs1}</div>
          <div style="float:right">${overs2}</div>
          <div style="clear:both"></div>
        </div>

        <div class="cric-status">${status}</div>
      </div>
    `;
  }

  async function loadTab(type) {
    const box = document.getElementById("scoreContent");
    box.innerHTML = "Loading...";

    try {
      const data = await fetchData(type);
      if (!data || !data.length) {
        box.innerHTML = "<div class='small-note'>No matches available.</div>";
        return;
      }

      // Show only first match like your design
      box.innerHTML = renderMatch(data[0]);

    } catch (e) {
      box.innerHTML = "<div class='small-note'>Unable to load data.</div>";
    }
  }

  // Tab buttons
  document.getElementById("tabLive").onclick = ()=>{ 
    activate("tabLive"); 
    loadTab("live"); 
  };
  document.getElementById("tabFixture").onclick = ()=>{ 
    activate("tabFixture"); 
    loadTab("fixture"); 
  };
  document.getElementById("tabResult").onclick = ()=>{ 
    activate("tabResult"); 
    loadTab("result"); 
  };

  function activate(id){
    document.querySelectorAll(".score-tabs div")
      .forEach(el => el.classList.remove("active"));
    document.getElementById(id).classList.add("active");
  }

  // initial load
  loadTab("live");

})();
</script>
</div>
 -->

<!-- <script src="https://cdorgapi.b-cdn.net/widgets/matchlist.js"></script> -->
		
<iframe src="https://cwidget.crictimes.org/?v=1.1&a=525252&bo=525252&sb=ffffff&c=000000&b=ffffff&tc=000000&dc=ffffff" style="width:100%;min-height: 180px;" frameborder="0" scrolling="yes"></iframe>


   
       

				
				
				
				
				
				
            </div>
        </div>

    </div> <!-- END .cm-posts -->

</section>

<?php
return ob_get_clean();
}
add_shortcode("sports_widget", "sports_widget_shortcode");





// ===============================
// SHORTCODE: Featured World + International Posts
// ===============================
function featured_world_posts_shortcode() {

    ob_start();

    // Query posts from World + International categories
    $args = array(
        'category_name'  => 'world,international',
        'posts_per_page' => 3,
    );

    $query = new WP_Query($args);

    ?>

    <style>
        .featured-container {
            display: flex;
            gap: 30px;
        }

        .featured-item {
            flex: 1;
            display: flex;
            flex-direction: column;
            background: #fff;
            border-radius: 6px;
        }

        /* Fixed height image container */
        .featured-img-wrap {
            width: 100%;
            height: 280px; /* adjust height here */
            overflow: hidden;
            border-radius: 6px;
        }

        .featured-img {
            width: 100%;
            height: 100%;
            object-fit: cover; /* ensures equal height */
        }

        .featured-title {
            margin: 12px 0 5px 0;
            font-size: 20px;
            font-weight: 600;
            flex-grow: 1;
        }

        .featured-meta {
            font-size: 14px;
            color: #666;
            margin-bottom: 10px;
        }
    </style>

    <section class="featured-news">
        <h2 class="section-title">World</h2>

        <div class="featured-container">

        <?php  
        if ($query->have_posts()):
            while ($query->have_posts()): $query->the_post(); 
        ?>
            
            <div class="featured-item">

                <div class="featured-img-wrap">
                    <a href="<?php the_permalink(); ?>">
                        <?php 
                        if (has_post_thumbnail()) {
                            the_post_thumbnail('large', ['class' => 'featured-img']);
                        } else {
                            echo '<img src="https://via.placeholder.com/600x400" class="featured-img" />';
                        }
                        ?>
                    </a>
                </div>

                <h3 class="featured-title"><?php the_title(); ?></h3>

                <p class="featured-meta">
                    <?php echo get_the_date(); ?> • 2 min read
                </p>

            </div>

        <?php 
            endwhile; 
        endif;
        wp_reset_postdata();
        ?>

        </div>
    </section>

    <?php
    return ob_get_clean();
}
add_shortcode('featured_world_posts', 'featured_world_posts_shortcode');



/* ===========================================
   SHORTCODE: India Web Stories (Auto image fallback)
   Usage: [india_webstories]
=========================================== */

function get_first_image_from_post($post_id) {
    $post = get_post($post_id);

    if (!$post) return false;

    // Find first <img> tag in content
    preg_match('/<img[^>]+src=[\'"]([^\'"]+)[\'"]/i', $post->post_content, $match);

    if (isset($match[1])) {
        return $match[1];
    }

    return false;
}

function india_webstories_shortcode() {

ob_start();
?>

<style>
/* Title Section */
.webstories-section {
    max-width: 1400px;
    margin: 40px auto;
}

.webstories-title {
    font-size: 36px;
    font-weight: 500;
    margin-bottom: 10px;
}

.webstories-title-line {
    width: 120px;
    height: 4px;
    background: #b6124b;
    margin-bottom: 30px;
}

/* Grid Layout */
.webstories-grid {
    display: grid;
    grid-template-columns: repeat(4, 1fr);
    gap: 25px;
}

/* Card */
.webstory-card {
    position: relative;
    height: 450px;
    border-radius: 6px;
    overflow: hidden;
    cursor: pointer;
}

.webstory-card img {
    width: 100%;
    height: 100%;
    object-fit: cover;
}

/* Gradient */
.webstory-overlay {
    position: absolute;
    bottom: 0;
    left: 0;
    width: 100%;
    padding: 12px 20px; /* reduced padding to remove gap */

    background: linear-gradient(
        to top,
        rgba(0,0,0,0.97) 0%,
        rgba(0,0,0,0.85) 25%,
        rgba(0,0,0,0.5) 55%,
        rgba(0,0,0,0.15) 80%,
        transparent 100%
    );

    display: flex;
    flex-direction: column;
    justify-content: flex-end;  /* pushes content fully down */
    color: #fff;
}

.webstory-overlay,
.webstory-title,
.webstory-date {
    color: #ffffff !important;
}

.webstory-title {
    font-size: 18px;
    font-weight: 700;
    margin: 0;
}

.webstory-date {
    font-size: 14px;
    opacity: 0.9;
    margin-top: 5px;
}

/* Mobile */
@media (max-width: 900px) {
    .webstories-grid {
        grid-template-columns: repeat(2, 1fr);
    }
}

@media (max-width: 500px) {
    .webstories-grid {
        grid-template-columns: repeat(1, 1fr);
    }
}
</style>

<section class="webstories-section">

    <h2 class="webstories-title">India</h2>
    <div class="webstories-title-line"></div>

    <div class="webstories-grid">

    <?php
    // Fetch ONLY India category posts
    $args = array(
        'category_name'  => 'india',
        'posts_per_page' => 4
    );

    $query = new WP_Query($args);

    if ($query->have_posts()):
        while ($query->have_posts()):
            $query->the_post();

            // 1. Try Featured Image
            $img = get_the_post_thumbnail_url(get_the_ID(), 'large');

            // 2. If no featured image → get first image from content
            if (!$img) {
                $img = get_first_image_from_post(get_the_ID());
            }

            // 3. Still nothing? → Use placeholder
            if (!$img) {
                $img = "https://via.placeholder.com/600x900";
            }
    ?>

        <a href="<?php the_permalink(); ?>" class="webstory-card">
            <img src="<?php echo esc_url($img); ?>" alt="">
            <div class="webstory-overlay">
                <h3 class="webstory-title"><?php the_title(); ?></h3>
                <p class="webstory-date"><?php echo get_the_date(); ?></p>
            </div>
        </a>

    <?php
        endwhile;
    endif;

    wp_reset_postdata();
    ?>

    </div>
</section>

<?php
return ob_get_clean();
}

add_shortcode('india_webstories', 'india_webstories_shortcode');


/* ======================================
   Helper: Get first image from post content
====================================== */
function get_first_image_from_post_latest($post_id) {
    $post = get_post($post_id);
    if(!$post) return false;

    preg_match('/<img[^>]+src=[\'"]([^\'"]+)[\'"]/i', $post->post_content, $match);

    return $match[1] ?? false;
}


/* ======================================
   SHORTCODE: Latest News Section (All Categories)
   Usage: [latest_news_section]
====================================== */
function latest_news_section_shortcode() {
ob_start();
?>

<style>
.latest-news-wrapper {
    max-width: 1400px;
    margin: 40px auto;
    display: grid;
    grid-template-columns: 2fr 1fr;
    gap: 40px;
}

/* Left big news */
.latest-big img {
    width: 100%;
    height: 450px;
    object-fit: cover;
    border-radius: 6px;
}

.latest-big-title {
    font-size: 26px;
    font-weight: 600;
    margin: 15px 0 15px 0;
}

.latest-big-excerpt {
    font-size: 16px;
    color: #444;
}

.latest-meta {
    color: #555;
    font-size: 14px;
}

/* Right list */
.latest-list-item {
    display: grid;
    grid-template-columns: 1fr 90px;
    gap: 15px;
    padding: 12px 0;
    border-bottom: 1px solid #eee;
}

.latest-list-item img {
    width: 100%;
    height: 70px;
    object-fit: cover;
    border-radius: 4px;
}

.latest-list-title {
    font-size: 14px;
    font-weight: 500;
    margin-bottom: 3px;
}

.latest-list-meta {
    font-size: 14px;
    color: #666;
}

@media (max-width: 900px) {
    .latest-news-wrapper {
        grid-template-columns: 1fr;
    }
}
</style>

<div class="latest-news-wrapper">

    <!-- LEFT: BIG NEWS -->
    <div class="latest-big">
        <?php
        $big_query = new WP_Query([
            'posts_per_page' => 1
        ]);

        if ($big_query->have_posts()):
            while ($big_query->have_posts()): $big_query->the_post();

                // 1️⃣ Featured image
                $big_img = get_the_post_thumbnail_url(get_the_ID(), 'large');

                // 2️⃣ If not found → first image from post content
                if (!$big_img) {
                    $big_img = get_first_image_from_post_latest(get_the_ID());
                }

                // 3️⃣ Still missing → placeholder
                if (!$big_img) {
                    $big_img = "https://via.placeholder.com/900x600";
                }
        ?>
        
        <a href="<?php the_permalink(); ?>">
            <img src="<?php echo esc_url($big_img); ?>">
        </a>

        <h2 class="latest-big-title"><a href="<?php the_permalink(); ?>"><?php the_title(); ?></a></h2>
        <p class="latest-big-excerpt"><?php echo wp_trim_words(get_the_excerpt(), 20); ?></p>
        <p class="latest-meta"><?php echo human_time_diff(get_the_time('U'), current_time('timestamp')); ?> ago • 3 min read</p>

        <?php
            endwhile;
        endif;
        wp_reset_postdata();
        ?>
    </div>

    <!-- RIGHT LIST -->
    <div class="latest-list">
        <?php
        $list_query = new WP_Query([
            'posts_per_page' => 5,
            'offset' => 1
        ]);

        if ($list_query->have_posts()):
            while ($list_query->have_posts()): $list_query->the_post();

                // 1️⃣ Featured image
                $small_img = get_the_post_thumbnail_url(get_the_ID(), 'medium');

                // 2️⃣ If missing → first image in content
                if (!$small_img) {
                    $small_img = get_first_image_from_post_latest(get_the_ID());
                }

                // 3️⃣ Still missing → placeholder
                if (!$small_img) {
                    $small_img = "https://via.placeholder.com/150x100";
                }
        ?>

        <a href="<?php the_permalink(); ?>" class="latest-list-item">
            <div>
                <h3 class="latest-list-title"><?php the_title(); ?></h3>
                <p class="latest-list-meta"><?php echo human_time_diff(get_the_time('U'), current_time('timestamp')); ?> ago • 1 min read</p>
            </div>
            <img src="<?php echo esc_url($small_img); ?>">
        </a>

        <?php
            endwhile;
        endif;
        wp_reset_postdata();
        ?>
    </div>

</div>

<?php
return ob_get_clean();
}
add_shortcode('latest_news_section', 'latest_news_section_shortcode');

// punjab___________________________________++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++

function get_second_image_from_post($post_id) {
    $post = get_post($post_id);
    if (!$post) return false;

    preg_match('/<img[^>]+src=[\'"]([^\'"]+)[\'"]/i', $post->post_content, $match);
    return $match[1] ?? false;
}

function punjab_widget_shortcode() {
    ob_start();

    /* -------------------------------
       QUERY: FEATURED (LEFT) POST
    -------------------------------- */
    $featured = new WP_Query([
        'category_name' => 'punjab',
        'posts_per_page' => 1
    ]);
    ?>

<style>

/* FIX IMAGE OVERFLOW — UNIVERSAL CONTROL */
.cm-first-post img,
.cm-posts .cm-post img {
    width: 100% !important;
    max-width: 100% !important;
    height: auto !important;
    object-fit: cover !important;
    display: block !important;
    border-radius: 10px;
}

/* FEATURED LEFT BIG IMAGE — SET FIXED HEIGHT */
.cm-first-post img {
    height: 320px !important;     /* adjust if needed */
}

/* RIGHT SMALL IMAGES — FIXED SIZE */
.cm-posts .cm-post img {
    width: 130px !important;
    height: 90px !important;
    object-fit: cover !important;
    flex-shrink: 0 !important;
}

/* MOBILE STYLE FIXES */
@media (max-width: 768px) {

    .cm-featured-posts {
        display: block !important;
    }

    .cm-first-post {
        width: 100% !important;
        margin-bottom: 20px !important;
    }

    .cm-posts {
        width: 100% !important;
        margin-top: 10px !important;
        display: block !important;
    }

/* UPDATED IMAGE HEIGHT (SMALLER) */
.cm-first-post img {
    height: 250px !important;
}

    .cm-posts .cm-post {
        display: flex !important;
        width: 100% !important;
        margin-bottom: 12px !important;
        border-radius: 10px !important;
        padding: 10px !important;
    }

    .cm-posts .cm-post img {
        width: 100px !important;
        height: 70px !important;
        object-fit: cover !important;
        border-radius: 8px !important;
    }

    .cm-post-categories a {
        padding: 3px 8px !important;
        font-size: 10px !important;
    }

    .live-score-box {
        margin-top: 20px !important;
        padding: 15px !important;
        border-radius: 12px !important;
    }

    .live-score-box iframe {
        height: 220px !important;
    }
}
</style>

<section id="sports-section" class="widget cm-featured-posts cm-featured-posts--style-1">

    <h3 class="cm-widget-title"><span>Punjab</span></h3>

    <!-- LEFT FEATURED POST -->
    <div class="cm-first-post">
        <?php while($featured->have_posts()) : $featured->the_post(); ?>
        <div class="cm-post">

            <!-- Thumbnail -->
            <a href="<?php the_permalink(); ?>">
                <?php
                // FALLBACK IMAGE LOGIC
                $img = get_the_post_thumbnail_url(get_the_ID(), 'colormag-featured-post-medium');
                if (!$img) $img = get_first_image_from_post(get_the_ID());
                if (!$img) $img = "https://via.placeholder.com/800x500";
                echo '<img src="'.esc_url($img).'" />';
                ?>
            </a>

            <div class="cm-post-content">

                <!-- CATEGORY LABELS -->
                <div class="cm-entry-header-meta">
                    <div class="cm-post-categories">
                        <?php
                        $categories = get_the_category();
                        foreach ($categories as $cat) {
                            echo '<a href="' . get_category_link($cat->term_id) . '" 
                                style="
                                    background:#000;
                                    color:#fff;
                                    padding:5px 10px;
                                    border-radius:4px;
                                    font-size:10px;
                                    font-weight:500;
                                    text-transform:uppercase;
                                    margin-right:6px;
                                    display:inline-block;
                                ">'. esc_html($cat->name) .'</a>';
                        }
                        ?>
                    </div>
                </div>

                <!-- TITLE -->
                <h3 class="cm-entry-title">
                    <a href="<?php the_permalink(); ?>">
                        <?php the_title(); ?>
                    </a>
                </h3>

                <!-- META -->
                <div class="cm-below-entry-meta">
                    <span class="cm-post-date">
                        <time><?php echo get_the_date(); ?></time>
                    </span>

                    <span class="cm-author">
                        <a><?php the_author(); ?></a>
                    </span>
                </div>

                <!-- EXCERPT -->
                <div class="cm-entry-summary">
                    <p><?php echo wp_trim_words(get_the_excerpt(), 20); ?></p>
                </div>

            </div>
        </div>
        <?php endwhile; wp_reset_postdata(); ?>
    </div>



    <!-- RIGHT-SIDE POSTS -->
    <div class="cm-posts">

        <?php
        $right_posts = new WP_Query([
            'category_name' => 'punjab',
            'posts_per_page' => 3,
            'offset' => 1
        ]);
        ?>

        <?php while($right_posts->have_posts()) : $right_posts->the_post(); ?>
        <div class="cm-post">

            <!-- Small Image -->
            <a href="<?php the_permalink(); ?>">
                <?php
                // FALLBACK IMAGE LOGIC
                $img = get_the_post_thumbnail_url(get_the_ID(), 'colormag-featured-post-small');
                if (!$img) $img = get_first_image_from_post(get_the_ID());
                if (!$img) $img = "https://via.placeholder.com/400x300";
                echo '<img src="'.esc_url($img).'" />';
                ?>
            </a>

            <div class="cm-post-content">

                <!-- CATEGORY BADGES -->
                <div class="cm-entry-header-meta">
                    <div class="cm-post-categories">
                        <?php
                        $categories = get_the_category();
                        foreach ($categories as $cat) {
                            echo '<a href="' . get_category_link($cat->term_id) . '" 
                                style="
                                    background:#000;
                                    color:#fff;
                                    padding:1px 4px;
                                    border-radius:3px;
                                    font-size:8px;
                                    font-weight:400;
                                    text-transform:uppercase;
                                    margin-right:5px;
                                    display:inline-block;
                                ">'. esc_html($cat->name) .'</a>';
                        }
                        ?>
                    </div>
                </div>

                <!-- Title -->
                <h3 class="cm-entry-title">
                    <a href="<?php the_permalink(); ?>">
                        <?php the_title(); ?>
                    </a>
                </h3>

                <!-- Meta -->
                <div class="cm-below-entry-meta">
                    <span class="cm-post-date">
                        <time><?php echo get_the_date(); ?></time>
                    </span>

                    <span class="cm-author">
                        <a><?php the_author(); ?></a>
                    </span>
                </div>

            </div>
        </div>
        <?php endwhile; wp_reset_postdata(); ?>

    </div> <!-- END .cm-posts -->

</section>

<?php
return ob_get_clean();
}
add_shortcode("punjab_widget", "punjab_widget_shortcode");


// job________________________________________



function get_third_image_from_post($post_id) {
    $post = get_post($post_id);
    if (!$post) return false;

    preg_match('/<img[^>]+src=[\'"]([^\'"]+)[\'"]/i', $post->post_content, $match);
    return $match[1] ?? false;
}

function job_widget_shortcode() {
    ob_start();

    /* -------------------------------
       QUERY: FEATURED (LEFT) POST
    -------------------------------- */
    $featured = new WP_Query([
        'category_name' => 'job',
        'posts_per_page' => 1
    ]);
    ?>

<style>
/* MOBILE STYLE FIXES */
@media (max-width: 768px) {

    .cm-featured-posts {
        display: block !important;
    }

    .cm-first-post {
        width: 100% !important;
        margin-bottom: 20px !important;
    }

    .cm-posts {
        width: 100% !important;
        margin-top: 10px !important;
        display: block !important;
    }

/* UPDATED IMAGE HEIGHT (SMALLER) */
.cm-first-post img {
    height: 250px !important;
}

    .cm-posts .cm-post {
        display: flex !important;
        width: 100% !important;
        margin-bottom: 12px !important;
        border-radius: 10px !important;
        padding: 10px !important;
    }

    .cm-posts .cm-post img {
        width: 100px !important;
        height: 70px !important;
        object-fit: cover !important;
        border-radius: 8px !important;
    }

    .cm-post-categories a {
        padding: 3px 8px !important;
        font-size: 10px !important;
    }

    .live-score-box {
        margin-top: 20px !important;
        padding: 15px !important;
        border-radius: 12px !important;
    }

    .live-score-box iframe {
        height: 220px !important;
    }
}
</style>

<section id="sports-section" class="widget cm-featured-posts cm-featured-posts--style-1">

    <h3 class="cm-widget-title"><span>Job</span></h3>

    <!-- LEFT FEATURED POST -->
    <div class="cm-first-post">
        <?php while($featured->have_posts()) : $featured->the_post(); ?>
        <div class="cm-post">

            <!-- Thumbnail -->
            <a href="<?php the_permalink(); ?>">
                <?php
                // FALLBACK IMAGE LOGIC
                $img = get_the_post_thumbnail_url(get_the_ID(), 'colormag-featured-post-medium');
                if (!$img) $img = get_first_image_from_post(get_the_ID());
                if (!$img) $img = "https://via.placeholder.com/800x500";
                echo '<img src="'.esc_url($img).'" />';
                ?>
            </a>

            <div class="cm-post-content">

                <!-- CATEGORY LABELS -->
                <div class="cm-entry-header-meta">
                    <div class="cm-post-categories">
                        <?php
                        $categories = get_the_category();
                        foreach ($categories as $cat) {
                            echo '<a href="' . get_category_link($cat->term_id) . '" 
                                style="
                                    background:#000;
                                    color:#fff;
                                    padding:5px 10px;
                                    border-radius:4px;
                                    font-size:10px;
                                    font-weight:500;
                                    text-transform:uppercase;
                                    margin-right:6px;
                                    display:inline-block;
                                ">'. esc_html($cat->name) .'</a>';
                        }
                        ?>
                    </div>
                </div>

                <!-- TITLE -->
                <h3 class="cm-entry-title">
                    <a href="<?php the_permalink(); ?>">
                        <?php the_title(); ?>
                    </a>
                </h3>

                <!-- META -->
                <div class="cm-below-entry-meta">
                    <span class="cm-post-date">
                        <time><?php echo get_the_date(); ?></time>
                    </span>

                    <span class="cm-author">
                        <a><?php the_author(); ?></a>
                    </span>
                </div>

                <!-- EXCERPT -->
                <div class="cm-entry-summary">
                    <p><?php echo wp_trim_words(get_the_excerpt(), 20); ?></p>
                </div>

            </div>
        </div>
        <?php endwhile; wp_reset_postdata(); ?>
    </div>



    <!-- RIGHT-SIDE POSTS -->
    <div class="cm-posts">

        <?php
        $right_posts = new WP_Query([
            'category_name' => 'job',
            'posts_per_page' => 3,
            'offset' => 1
        ]);
        ?>

        <?php while($right_posts->have_posts()) : $right_posts->the_post(); ?>
        <div class="cm-post">

            <!-- Small Image -->
            <a href="<?php the_permalink(); ?>">
                <?php
                // FALLBACK IMAGE LOGIC
                $img = get_the_post_thumbnail_url(get_the_ID(), 'colormag-featured-post-small');
                if (!$img) $img = get_first_image_from_post(get_the_ID());
                if (!$img) $img = "https://via.placeholder.com/400x300";
                echo '<img src="'.esc_url($img).'" />';
                ?>
            </a>

            <div class="cm-post-content">

                <!-- CATEGORY BADGES -->
                <div class="cm-entry-header-meta">
                    <div class="cm-post-categories">
                        <?php
                        $categories = get_the_category();
                        foreach ($categories as $cat) {
                            echo '<a href="' . get_category_link($cat->term_id) . '" 
                                style="
                                    background:#000;
                                    color:#fff;
                                    padding:1px 4px;
                                    border-radius:3px;
                                    font-size:8px;
                                    font-weight:400;
                                    text-transform:uppercase;
                                    margin-right:5px;
                                    display:inline-block;
                                ">'. esc_html($cat->name) .'</a>';
                        }
                        ?>
                    </div>
                </div>

                <!-- Title -->
                <h3 class="cm-entry-title">
                    <a href="<?php the_permalink(); ?>">
                        <?php the_title(); ?>
                    </a>
                </h3>

                <!-- Meta -->
                <div class="cm-below-entry-meta">
                    <span class="cm-post-date">
                        <time><?php echo get_the_date(); ?></time>
                    </span>

                    <span class="cm-author">
                        <a><?php the_author(); ?></a>
                    </span>
                </div>

            </div>
        </div>
        <?php endwhile; wp_reset_postdata(); ?>

    </div> <!-- END .cm-posts -->

</section>

<?php
return ob_get_clean();
}
add_shortcode("job_widget", "job_widget_shortcode");


// -------------------------------education

function get_third2_image_from_post($post_id) {
    $post = get_post($post_id);
    if (!$post) return false;

    preg_match('/<img[^>]+src=[\'"]([^\'"]+)[\'"]/i', $post->post_content, $match);
    return $match[1] ?? false;
}

function education_widget_shortcode() {
    ob_start();

    /* -------------------------------
       QUERY: FEATURED (LEFT) POST
    -------------------------------- */
    $featured = new WP_Query([
        'category_name' => 'education',
        'posts_per_page' => 1
    ]);
    ?>

<style>
/* MOBILE STYLE FIXES */
@media (max-width: 768px) {

    .cm-featured-posts {
        display: block !important;
    }

    .cm-first-post {
        width: 100% !important;
        margin-bottom: 20px !important;
    }

    .cm-posts {
        width: 100% !important;
        margin-top: 10px !important;
        display: block !important;
    }

/* UPDATED IMAGE HEIGHT (SMALLER) */
.cm-first-post img {
    height: 250px !important;
}

    .cm-posts .cm-post {
        display: flex !important;
        width: 100% !important;
        margin-bottom: 12px !important;
        border-radius: 10px !important;
        padding: 10px !important;
    }

    .cm-posts .cm-post img {
        width: 100px !important;
        height: 70px !important;
        object-fit: cover !important;
        border-radius: 8px !important;
    }

    .cm-post-categories a {
        padding: 3px 8px !important;
        font-size: 10px !important;
    }

    .live-score-box {
        margin-top: 20px !important;
        padding: 15px !important;
        border-radius: 12px !important;
    }

    .live-score-box iframe {
        height: 220px !important;
    }
}
</style>

<section id="sports-section" class="widget cm-featured-posts cm-featured-posts--style-1">

    <h3 class="cm-widget-title"><span>Education</span></h3>

    <!-- LEFT FEATURED POST -->
    <div class="cm-first-post">
        <?php while($featured->have_posts()) : $featured->the_post(); ?>
        <div class="cm-post">

            <!-- Thumbnail -->
            <a href="<?php the_permalink(); ?>">
                <?php
                // FALLBACK IMAGE LOGIC
                $img = get_the_post_thumbnail_url(get_the_ID(), 'colormag-featured-post-medium');
                if (!$img) $img = get_first_image_from_post(get_the_ID());
                if (!$img) $img = "https://via.placeholder.com/800x500";
                echo '<img src="'.esc_url($img).'" />';
                ?>
            </a>

            <div class="cm-post-content">

                <!-- CATEGORY LABELS -->
                <div class="cm-entry-header-meta">
                    <div class="cm-post-categories">
                        <?php
                        $categories = get_the_category();
                        foreach ($categories as $cat) {
                            echo '<a href="' . get_category_link($cat->term_id) . '" 
                                style="
                                    background:#000;
                                    color:#fff;
                                    padding:5px 10px;
                                    border-radius:4px;
                                    font-size:10px;
                                    font-weight:500;
                                    text-transform:uppercase;
                                    margin-right:6px;
                                    display:inline-block;
                                ">'. esc_html($cat->name) .'</a>';
                        }
                        ?>
                    </div>
                </div>

                <!-- TITLE -->
                <h3 class="cm-entry-title">
                    <a href="<?php the_permalink(); ?>">
                        <?php the_title(); ?>
                    </a>
                </h3>

                <!-- META -->
                <div class="cm-below-entry-meta">
                    <span class="cm-post-date">
                        <time><?php echo get_the_date(); ?></time>
                    </span>

                    <span class="cm-author">
                        <a><?php the_author(); ?></a>
                    </span>
                </div>

                <!-- EXCERPT -->
                <div class="cm-entry-summary">
                    <p><?php echo wp_trim_words(get_the_excerpt(), 20); ?></p>
                </div>

            </div>
        </div>
        <?php endwhile; wp_reset_postdata(); ?>
    </div>



    <!-- RIGHT-SIDE POSTS -->
    <div class="cm-posts">

        <?php
        $right_posts = new WP_Query([
            'category_name' => 'education',
            'posts_per_page' => 3,
            'offset' => 1
        ]);
        ?>

        <?php while($right_posts->have_posts()) : $right_posts->the_post(); ?>
        <div class="cm-post">

            <!-- Small Image -->
            <a href="<?php the_permalink(); ?>">
                <?php
                // FALLBACK IMAGE LOGIC
                $img = get_the_post_thumbnail_url(get_the_ID(), 'colormag-featured-post-small');
                if (!$img) $img = get_first_image_from_post(get_the_ID());
                if (!$img) $img = "https://via.placeholder.com/400x300";
                echo '<img src="'.esc_url($img).'" />';
                ?>
            </a>

            <div class="cm-post-content">

                <!-- CATEGORY BADGES -->
                <div class="cm-entry-header-meta">
                    <div class="cm-post-categories">
                        <?php
                        $categories = get_the_category();
                        foreach ($categories as $cat) {
                            echo '<a href="' . get_category_link($cat->term_id) . '" 
                                style="
                                    background:#000;
                                    color:#fff;
                                    padding:1px 4px;
                                    border-radius:3px;
                                    font-size:8px;
                                    font-weight:400;
                                    text-transform:uppercase;
                                    margin-right:5px;
                                    display:inline-block;
                                ">'. esc_html($cat->name) .'</a>';
                        }
                        ?>
                    </div>
                </div>

                <!-- Title -->
                <h3 class="cm-entry-title">
                    <a href="<?php the_permalink(); ?>">
                        <?php the_title(); ?>
                    </a>
                </h3>

                <!-- Meta -->
                <div class="cm-below-entry-meta">
                    <span class="cm-post-date">
                        <time><?php echo get_the_date(); ?></time>
                    </span>

                    <span class="cm-author">
                        <a><?php the_author(); ?></a>
                    </span>
                </div>

            </div>
        </div>
        <?php endwhile; wp_reset_postdata(); ?>

    </div> <!-- END .cm-posts -->

</section>

<?php
return ob_get_clean();
}
add_shortcode("education_widget", "education_widget_shortcode");


// ott-----------------------------------------------


function get_third3_image_from_post($post_id) {
    $post = get_post($post_id);
    if (!$post) return false;

    preg_match('/<img[^>]+src=[\'"]([^\'"]+)[\'"]/i', $post->post_content, $match);
    return $match[1] ?? false;
}

function ott_widget_shortcode() {
    ob_start();

    /* -------------------------------
       QUERY: FEATURED (LEFT) POST
    -------------------------------- */
    $featured = new WP_Query([
        'category_name' => 'ott',
        'posts_per_page' => 1
    ]);
    ?>

<style>
/* MOBILE STYLE FIXES */
@media (max-width: 768px) {

    .cm-featured-posts {
        display: block !important;
    }

    .cm-first-post {
        width: 100% !important;
        margin-bottom: 20px !important;
    }

    .cm-posts {
        width: 100% !important;
        margin-top: 10px !important;
        display: block !important;
    }

/* UPDATED IMAGE HEIGHT (SMALLER) */
.cm-first-post img {
    height: 250px !important;
}

    .cm-posts .cm-post {
        display: flex !important;
        width: 100% !important;
        margin-bottom: 12px !important;
        border-radius: 10px !important;
        padding: 10px !important;
    }

    .cm-posts .cm-post img {
        width: 100px !important;
        height: 70px !important;
        object-fit: cover !important;
        border-radius: 8px !important;
    }

    .cm-post-categories a {
        padding: 3px 8px !important;
        font-size: 10px !important;
    }

    .live-score-box {
        margin-top: 20px !important;
        padding: 15px !important;
        border-radius: 12px !important;
    }

    .live-score-box iframe {
        height: 220px !important;
    }
}
</style>

<section id="sports-section" class="widget cm-featured-posts cm-featured-posts--style-1">

    <h3 class="cm-widget-title"><span>Ott</span></h3>

    <!-- LEFT FEATURED POST -->
    <div class="cm-first-post">
        <?php while($featured->have_posts()) : $featured->the_post(); ?>
        <div class="cm-post">

            <!-- Thumbnail -->
            <a href="<?php the_permalink(); ?>">
                <?php
                // FALLBACK IMAGE LOGIC
                $img = get_the_post_thumbnail_url(get_the_ID(), 'colormag-featured-post-medium');
                if (!$img) $img = get_first_image_from_post(get_the_ID());
                if (!$img) $img = "https://via.placeholder.com/800x500";
                echo '<img src="'.esc_url($img).'" />';
                ?>
            </a>

            <div class="cm-post-content">

                <!-- CATEGORY LABELS -->
                <div class="cm-entry-header-meta">
                    <div class="cm-post-categories">
                        <?php
                        $categories = get_the_category();
                        foreach ($categories as $cat) {
                            echo '<a href="' . get_category_link($cat->term_id) . '" 
                                style="
                                    background:#000;
                                    color:#fff;
                                    padding:5px 10px;
                                    border-radius:4px;
                                    font-size:10px;
                                    font-weight:500;
                                    text-transform:uppercase;
                                    margin-right:6px;
                                    display:inline-block;
                                ">'. esc_html($cat->name) .'</a>';
                        }
                        ?>
                    </div>
                </div>

                <!-- TITLE -->
                <h3 class="cm-entry-title">
                    <a href="<?php the_permalink(); ?>">
                        <?php the_title(); ?>
                    </a>
                </h3>

                <!-- META -->
                <div class="cm-below-entry-meta">
                    <span class="cm-post-date">
                        <time><?php echo get_the_date(); ?></time>
                    </span>

                    <span class="cm-author">
                        <a><?php the_author(); ?></a>
                    </span>
                </div>

                <!-- EXCERPT -->
                <div class="cm-entry-summary">
                    <p><?php echo wp_trim_words(get_the_excerpt(), 20); ?></p>
                </div>

            </div>
        </div>
        <?php endwhile; wp_reset_postdata(); ?>
    </div>



    <!-- RIGHT-SIDE POSTS -->
    <div class="cm-posts">

        <?php
        $right_posts = new WP_Query([
            'category_name' => 'ott',
            'posts_per_page' => 3,
            'offset' => 1
        ]);
        ?>

        <?php while($right_posts->have_posts()) : $right_posts->the_post(); ?>
        <div class="cm-post">

            <!-- Small Image -->
            <a href="<?php the_permalink(); ?>">
                <?php
                // FALLBACK IMAGE LOGIC
                $img = get_the_post_thumbnail_url(get_the_ID(), 'colormag-featured-post-small');
                if (!$img) $img = get_first_image_from_post(get_the_ID());
                if (!$img) $img = "https://via.placeholder.com/400x300";
                echo '<img src="'.esc_url($img).'" />';
                ?>
            </a>

            <div class="cm-post-content">

                <!-- CATEGORY BADGES -->
                <div class="cm-entry-header-meta">
                    <div class="cm-post-categories">
                        <?php
                        $categories = get_the_category();
                        foreach ($categories as $cat) {
                            echo '<a href="' . get_category_link($cat->term_id) . '" 
                                style="
                                    background:#000;
                                    color:#fff;
                                    padding:1px 4px;
                                    border-radius:3px;
                                    font-size:8px;
                                    font-weight:400;
                                    text-transform:uppercase;
                                    margin-right:5px;
                                    display:inline-block;
                                ">'. esc_html($cat->name) .'</a>';
                        }
                        ?>
                    </div>
                </div>

                <!-- Title -->
                <h3 class="cm-entry-title">
                    <a href="<?php the_permalink(); ?>">
                        <?php the_title(); ?>
                    </a>
                </h3>

                <!-- Meta -->
                <div class="cm-below-entry-meta">
                    <span class="cm-post-date">
                        <time><?php echo get_the_date(); ?></time>
                    </span>

                    <span class="cm-author">
                        <a><?php the_author(); ?></a>
                    </span>
                </div>

            </div>
        </div>
        <?php endwhile; wp_reset_postdata(); ?>

    </div> <!-- END .cm-posts -->

</section>

<?php
return ob_get_clean();
}


add_shortcode("ott_widget", "ott_widget_shortcode");


