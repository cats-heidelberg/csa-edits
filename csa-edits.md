# Theme and plugin edits

## Filter for Lernmaterialien

BuddyBoss Theme: learndash-helper.php (inc/plugins/learndash-helper.php)

	case 'recent': hinzufügen

	case 'recent':
					$query_order_by = 'date';
					$query_order    = 'desc';
					break;

## Sidebar htmlentities

BuddyBoss Theme: learndash-sidebar.php (learndash/ld30/learndash-sidebar.php)

	<a href="<?php echo get_permalink( $lesson->ID ); ?>" title="<?php echo htmlEntities($lesson->post_title); ?>" class="bb-lesson-head flex">
	
	<h2 class="course-entry-title"><?php echo $parent_course_title; ?></h2>

### Fix missing title with trailing exams

	<span class="flex-1 bb-lms-title <?php echo esc_attr( learndash_is_quiz_complete( $user_id, $lesson_quiz['post']->ID, $course_id ) ? 'bb-completed-item' : 'bb-not-completed-item' ); ?>">
	<?php echo $lesson_quiz['post']->post_title; ?>
	</span>

## Course placeholders

learndash/ld30/course.php

	esc_html_x( 'Inhalt', 'placeholder: Course', 'buddyboss-theme' ),


## Enable excerpts for LearnDash Topics

BuddyBoss Theme: Theme-Funktionen (functions.php)

	function add_excerpts_to_ld_topic() {
		add_post_type_support( 'sfwd-topic', 'excerpt' );
	}
	add_action( 'init', 'add_excerpts_to_ld_topic' );



### Fix for random pagination issues

	session_start();

	add_filter('posts_orderby', 'edit_posts_orderby');

	function edit_posts_orderby($orderby_statement) {

	if (is_page('einzelmaterialien') || is_page('links')) {
		$seed = $_SESSION['seed'];
		if (empty($seed)) {
		$seed = rand();
		$_SESSION['seed'] = $seed;
		}

		$orderby_statement = 'RAND('.$seed.')';
	}
		return $orderby_statement;
	}


## Zotpress

zotpress/js/zotpress.shortcode.bib.min.js (aktiv)

	// Add abstracts, if any
	
		if ( params.zpShowAbstracts == true &&
		( item.data.hasOwnProperty('abstractNote') && item.data.abstractNote.length > 0 ) )
		tempItem +="<p class='zp-Abstract'><span class='zp-Abstract-Title'>Abstract:</span> " +item.data.abstractNote+ "</p>\n";
								
	// Add notes, if any
		if ( params.zpShowNotes == true && item.hasOwnProperty('notes') )
		tempItem +="<div class='zp-Citation-Notes' tabindex='0'>" +item.notes+ "</div>\n";

	// Add tags, if any

zotpress/lib/shortcode/shortcode.request.php (aktiv)

		$zp_target_output = "target='_blank' ";

zotpress/lib/shortcode/shortcode.request.php

	Cite -> Zitieren

## Elementor Pro

elementor-pro/modules/posts/skins/skin-cards.php

	protected function render_badge() {
			$taxonomy = $this->get_instance_value( 'badge_taxonomy' );
			if ( empty( $taxonomy ) || ! taxonomy_exists( $taxonomy ) ) {
				return;
			}

			$terms = get_the_terms( get_the_ID(), $taxonomy );
			if ( ! is_array( $terms ) ) {
				return;
			}
			?>
			<div class="elementor-post__badges"><?php
				$i=1;
				foreach( array_slice($terms,0,3) as $term ) : ?>
					<div class="elementor-post__badge elementor-post__badge<?php echo $i; ?>"><?php echo $term->name; ?></div>
				
				<?php 
			$i+=1; 
			endforeach; ?>
				</div>
			<?php
		}

##  Events Widget 

events-widgets-for-elementor-and-the-events-calendar/widgets/layouts/ectbe-list.php

	$events_html .='<h2 class="ectbe-list-title"><a href="'.esc_url( $url).'">'.$event_title.'</a></h2>

	// Remove this! //

	elseif ( $style == 'style-1' ) {
					$events_html .= $ectbe_read_more;
				}


## Events Calendar 

 the-events-calendar/src/views/v2/list/event/date-tag.php 


	$event_month_title  = $display_date->format_i18n( 'M' );
	$event_day_num   = $display_date->format_i18n( 'j' );
	$event_date_attr = $display_date->format( Dates::DBDATEFORMAT );
	?>
	<div class="tribe-events-calendar-list__event-date-tag tribe-common-g-col">
		<time class="tribe-events-calendar-list__event-date-tag-datetime" datetime="<?php echo esc_attr( $event_date_attr ); ?>" aria-hidden="true">
			<span class="tribe-events-calendar-list__event-date-tag-weekday">
				<?php echo esc_html( $event_month_title ); ?>
			</span>
			<span class="tribe-events-calendar-list__event-date-tag-daynum tribe-common-h5 tribe-common-h4--min-medium">
				<?php echo esc_html( $event_day_num ); ?>
			</span>
		</time>
	</div>

## Taxopress 

	<a href="%tag_link%" id="tag-link-%tag_id%" class="st-tags t%tag_scale%" title="%tag_count% topics" %tag_rel% style="%tag_size% %tag_color%">%tag_name%</a>

## H5P 

h5pmods-wordpress-plugin-master/styles/general.css 

	.h5p-inner {
	border: 1px solid #999 !important;
	}