# Theme and plugin edits

# Plugins

## Elementor Pro

_Replace "render_badge" function in order to display multiple badges._

-> elementor-pro/modules/posts/skins/skin-cards.php

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
  
##  Events Widgets For Elementor And The Events Calendar

_Remove "read more" button in the calendar widget on the main page._

-> events-widgets-for-elementor-and-the-events-calendar/widgets/layouts/ectbe-list.php

	// Remove this! //

	elseif ( $style == 'style-1' ) {
					$events_html .= $ectbe_read_more;
				}


## Events Calendar 

_Replace date/time format to display months._

-> the-events-calendar/src/views/v2/list/event/date-tag.php 


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

## H5P 

h5pmods-wordpress-plugin-master/styles/general.css 

	.h5p-inner {
	border: 1px solid #999 !important;
	}

## Zotpress

_Adding notes to the zotpress items._

-> zotpress/js/zotpress.shortcode.bib.min.js (aktiv)

	// Add abstracts, if any
	
		if ( params.zpShowAbstracts == true &&
		( item.data.hasOwnProperty('abstractNote') && item.data.abstractNote.length > 0 ) )
		tempItem +="<p class='zp-Abstract'><span class='zp-Abstract-Title'>Abstract:</span> " +item.data.abstractNote+ "</p>\n";
								
	// Add this part -->  Add notes, if any 
		if ( params.zpShowNotes == true && item.hasOwnProperty('notes') )
		tempItem +="<div class='zp-Citation-Notes' tabindex='0'>" +item.notes+ "</div>\n";

	// Add tags, if any

_Open notes links in new tabs._

-> zotpress/lib/shortcode/shortcode.request.php (aktiv)

		$zp_target_output = "target='_blank' ";

-> zotpress/lib/shortcode/shortcode.request.php

_Replace "Cite" mit "Zitieren"._
_Add notes to the citations._


	// Display notes, if any

							if ( count($zp_notes_meta) == 1 && (strpos($zp_notes_meta[0], 'privat') === false))
							{
								$temp_notes = "<li id=\"zp-Note-".$item->key."\">\n";
								$temp_notes .= $zp_notes_meta[0]."\n";
                                	// Add to item
								$item->notes = $temp_notes . "</li>\n";

					// Add note reference to citation
								$note_class = "zp-Notes-Reference"; if ( is_admin_bar_showing() ) $note_class .= " zp-Admin-Bar-Showing";
								$item->bib = preg_replace('~(.*)' . preg_quote('</div>', '~') . '(.*?)~', '$1' . " <sup class=\"".$note_class."\"><a href=\"#zp-Note-".$item->key."\">".$zp_notes_num."</a></sup> </div>" . '$2', $item->bib, 1);
								$zp_notes_num++;
                            } else if ( count($zp_notes_meta) > 1) // multiple

								{
                                    $temp_notes = "<li id=\"zp-Note-".$item->key."\">\n";
									$temp_notes .= "<ul class='zp-Citation-Item-Notes'>\n";

									foreach ($zp_notes_meta as $zp_note_meta)
                                    if (strpos($zp_note_meta, '[privat]') === false)
										$temp_notes .= "<li class='zp-Citation-note'>" . $zp_note_meta . "\n</li>\n";

									$temp_notes .= "\n</ul><!-- .zp-Citation-Item-Notes -->\n\n";
                                    	// Add to item
								$item->notes = $temp_notes . "</li>\n";

					// Add note reference to citation
								$note_class = "zp-Notes-Reference"; if ( is_admin_bar_showing() ) $note_class .= " zp-Admin-Bar-Showing";
								$item->bib = preg_replace('~(.*)' . preg_quote('</div>', '~') . '(.*?)~', '$1' . " <sup class=\"".$note_class."\"><a href=\"#zp-Note-".$item->key."\">".$zp_notes_num."</a></sup> </div>" . '$2', $item->bib, 1);
								$zp_notes_num++;
								}
						} // Children exist; not "Item not found"
					} // Check if item has children
				} // $zpr["downloadable"]
				



# Theme
## Filter for Lernmaterialien

_Filter "Lernmodule" by date. Add case and replace alphabetical._

-> BuddyBoss Theme: inc/plugins/learndash-helper.php

	BuddyBossTheme/Learndash/Archive/DefaultOrderBy', 'alphabetical' (replace with 'recent')

	(case 'recent': hinzufügen)

	case 'recent':
					$query_order_by = 'date';
					$query_order    = 'desc';
					break;





## Enable excerpts for LearnDash Topics

-> BuddyBoss Theme: Theme-Funktionen (functions.php)

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


# Deprecated fixes
## Sidebar htmlentities

BuddyBoss Theme: learndash-sidebar.php (learndash/ld30/learndash-sidebar.php)

	<a href="<?php echo get_permalink( $lesson->ID ); ?>" title="<?php echo htmlEntities($lesson->post_title); ?>" class="bb-lesson-head flex">
	
	<h2 class="course-entry-title"><?php echo $parent_course_title; ?></h2>

#### Fix missing title with trailing exams

	<span class="flex-1 bb-lms-title <?php echo esc_attr( learndash_is_quiz_complete( $user_id, $lesson_quiz['post']->ID, $course_id ) ? 'bb-completed-item' : 'bb-not-completed-item' ); ?>">
	<?php echo $lesson_quiz['post']->post_title; ?>
	</span>

 ## Course placeholders

_Rename Course to Inhalt on Lernmodule pages._

-> learndash/ld30/course.php

	esc_html_x( 'Inhalt', 'placeholder: Course', 'buddyboss-theme' ),
