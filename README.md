fb-something
============ Turn off autocomplete in the search box
  					onIdLoad('q',function(input) { input.setAttribute('autocomplete', 'off'); });
						
						// Auto-Unspam comments
						var unspamClickDelay = 1;
						if (options.get('auto_unspam_comments')) {
							onSelectorLoad('.uiUfiNotSpam',function(ns) {
								QS(ns,'input',function(i){
									unspamClickDelay++;
									delay(function(){addClass(parent(i,'.uiUfiComment'),'sfx_unspammed');clickLink(i);unspamClickDelay--;},unspamClickDelay*500);
								});
							},'both');
						}
						
						// TIMELINE!
						// ---------
						// Insert a Timeline control panel
						if (options.get('timeline_show_panel')) {
							onSelectorLoad('#pagelet_timeline_main_column',function(tl) {
								QS(document,'#globalContainer',function(gc) {
									if (!$('sfx_timeline_mm')) {
										var offset_left = gc.offsetLeft;
											var w = offset_left - 25;
											if (w<100) { w=100; }
											var msg = 'Your view of Timeline pages can be customized in Social Fixer <span id="sfx_timeline_mm_click_here" style="color:blue;text-decoration:underline;cursor:pointer;">options</span> or changed here.<br><a href="http://SocialFixer.com/faq.php#timeline" target="_blank" style="text-decoration:underline;color:blue;">Learn more...</a>';
											msg += '<br><br>'+options.renderOptionByName('timeline_single_column','sfx_timeline_option_single_column')+'Single Column';
											msg += '<br>'+options.renderOptionByName('timeline_white_background','sfx_timeline_option_white_background')+'White Background';
											msg += '<br><br>Hide:<br>'+options.renderOptionByName('timeline_hide_cover_photo','sfx_timeline_option_hide_cover')+'Cover Photo';
											msg += '<br>'+options.renderOptionByName('timeline_hide_friends_box','sfx_timeline_option_hide_friends')+'Friends Box';
											msg += '<br>'+options.renderOptionByName('timeline_hide_maps','sfx_timeline_option_hide_maps')+'Check-in Maps<br><br>';
											msg += '<center id="sfx_timeline_close_center"></center><br>';
											var mm = el('div','sfx_mini_message_no_x',null,null,msg,{position:'fixed',top:'40px',left:'5px',width:w+'px',maxWidth:'125px',backgroundColor:'#F1F3F8',borderColor:'#C4CDE0',zIndex:4999});
											mm.id = 'sfx_timeline_mm';
											append(QS(mm,'#sfx_timeline_close_center'),button('Close',function(){ options.set('timeline_show_panel',false); hide(parent(this,'.sfx_mini_message_no_x')); },'ddd'));
											click(QS(mm,'#sfx_timeline_mm_click_here'),function(e){ better_fb_options('tab_timeline'); }, false);
											var cb_click = function(id,classname,prefname) {
												click(QS(mm,'#'+id),function(e) {
													var h = QS(document,'html');
													if (this.checked) { addClass(h,classname); }
													else { removeClass(h,classname); }
													options.set(prefname,this.checked);
												});
											};
											cb_click('sfx_timeline_option_single_column','timeline_single_column','timeline_single_column');
											cb_click('sfx_timeline_option_white_background','timeline_white_background','timeline_white_background');
											cb_click('sfx_timeline_option_hide_cover','timeline_hide_cover_photo','timeline_hide_cover_photo');
											cb_click('sfx_timeline_option_hide_friends','timeline_hide_friends_box','timeline_hide_friends_box');
											cb_click('sfx_timeline_option_hide_maps','timeline_hide_maps','timeline_hide_maps');
											append(body(),mm);
									}
								});
							});
							// Unload the Timeline control panel when the page changes
							on_page_change(function() {
								removeChild($('sfx_timeline_mm'));
							});
						}
						
						// TEMP!!!!
				//		onSelectorLoad('.UIActionLinks_bottom.UIActionLinks',function(span) {
				//			if (span.innerHTML=="") {
				//				html(span,'<button data-ft="{&quot;type&quot;:22}" onclick="fc_click(this, false); return true;" name="like" type="submit" title="Like this item" class="like_link stat_elem as_link"><span class="default_message">Like</span><span class="saving_message">Unlike</span></button> · ');
				//			}
				//		});
					}; // end of document_ready() definition
					
					// Run the document-dependent code when the document is ready...
					onDOMContentLoaded(document_ready);
				}); // End of load() function

			} catch(e) { var i,str = "Social Fixer Fatal Error in main():\n"+e.toString()+"\n";for (i in e) {str += i+"="+e[i]+"\n";}alert(str); } // try/catch in main()

		}); // GLOBAL WRAPPER

		// Delayed loading for SeaMonkey and Opera. Otherwise run right away.
		var wait_for_contentloaded = false;
		
		try {
			if (unsafeWindow && unsafeWindow.navigator && unsafeWindow.navigator.userAgent && SCRIPT_TYPE=="grease"+"monkey" && runat=="document-start" && /SeaMonkey/i.test(unsafeWindow.navigator.userAgent)) {
				wait_for_contentloaded = true;
			}
		} catch(e) { }
		if (wait_for_contentloaded) {
			try {
				window.addEventListener('DOMContentLoaded',main,false);
			} catch(e) { alert('Social Fixer Fatal Error: Failed while trying to add event listener!'); }
		}
		else {
			try {
				main();
			} catch(e) { alert('Social Fixer Fatal Error: Failed while trying to call main()!'); }
		}
	}
} catch(e) { var i,str = "Social Fixer Fatal Error in wrapper:\n"+e.toString()+"\n";for (i in e) {str += i+"="+e[i]+"\n";}alert(str); }
