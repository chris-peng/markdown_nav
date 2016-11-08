# markdown自动生成导航目录
把这一段代码插入到markdown生成的HTML文件的head标签中，将会自动根据markdown的标题按级别生成导航目录\([示例页面](https://chris-peng.github.io/markdown_nav/%E7%A4%BA%E4%BE%8B.html)\)：

	<script src="http://code.jquery.com/jquery-1.7.2.min.js"></script>
    <script type="text/javascript">
     $(document).ready(function(){

        var h1s = $("body").find("h1");
        var h2s = $("body").find("h2");
        var h3s = $("body").find("h3");
        var h4s = $("body").find("h4");
        var h5s = $("body").find("h5");
        var h6s = $("body").find("h6");

        var headCounts = [h1s.length, h2s.length, h3s.length, h4s.length, h5s.length, h6s.length];
        var vH1Tag = null;
        var vH2Tag = null;
        for(var i = 0; i < headCounts.length; i++){
            if(headCounts[i] > 0){
                if(vH1Tag == null){
                    vH1Tag = 'h' + (i + 1);
                }else{
                    vH2Tag = 'h' + (i + 1);
                }
            }
        }
        if(vH1Tag == null){
            return;
        }

        $("body").prepend('<div class="BlogAnchor">' + 
			'<span style="color:red;position:absolute;top:-6px;left:0px;cursor:pointer;" onclick="$(\'.BlogAnchor\').hide();">×</span>' +
            '<p>' + 
                '<b id="AnchorContentToggle" title="收起" style="cursor:pointer;">目录▲</b>' + 
            '</p>' + 
            '<div class="AnchorContent" id="AnchorContent"> </div>' + 
        '</div>' );

        var vH1Index = 0;
        var vH2Index = 0;
        $("body").find("h1,h2,h3,h4,h5,h6").each(function(i,item){
            var id = '';
            var name = '';
            var tag = $(item).get(0).tagName.toLowerCase();
            var className = '';
            if(tag == vH1Tag){
                id = name = ++vH1Index;
                name = id;
                vH2Index = 0;
                className = 'item_h1';
            }else if(tag == vH2Tag){
                id = vH1Index + '_' + ++vH2Index;
                name = vH1Index + '.' + vH2Index;
                className = 'item_h2';
            }
            $(item).attr("id","wow"+id);
			$(item).addClass("wow_head");
            $("#AnchorContent").css('max-height', ($(window).height() - 180) + 'px');
            $("#AnchorContent").append('<li><a class="nav_item '+className+' anchor-link" onclick="return false;" href="#" link="#wow'+id+'">'+name+" · "+$(this).text()+'</a></li>');
        });

        $("#AnchorContentToggle").click(function(){
            var text = $(this).html();
            if(text=="目录▲"){
                $(this).html("目录▼");
                $(this).attr({"title":"展开"});
            }else{
                $(this).html("目录▲");
                $(this).attr({"title":"收起"});
            }
            $("#AnchorContent").toggle();
        });
        $(".anchor-link").click(function(){
            $("html,body").animate({scrollTop: $($(this).attr("link")).offset().top}, 500);
        });
		
		var headerNavs = $(".BlogAnchor li .nav_item");
		var headerTops = [];
		$(".wow_head").each(function(i, n){
			headerTops.push($(n).offset().top);
		});
		$(window).scroll(function(){
			var scrollTop = $(window).scrollTop();
			$.each(headerTops, function(i, n){
				var distance = n - scrollTop;
				if(distance >= 0){
					$(".BlogAnchor li .nav_item.current").removeClass('current');
					$(headerNavs[i]).addClass('current');
					return false;
				}
			});
		});
     });
    </script>
    <style>
        /*导航*/
        .BlogAnchor {
            background: #f1f1f1;
            padding: 10px;
            line-height: 180%;
            position: fixed;
            right: 48px;
            top: 48px;
            border: 1px solid #aaaaaa;
        }
        .BlogAnchor p {
            font-size: 18px;
            color: #15a230;
            margin: 0 0 0.3rem 0;
            text-align: right;
        }
        .BlogAnchor .AnchorContent {
            padding: 5px 0px;
            overflow: auto;
        }
        .BlogAnchor li{
            text-indent: 0.5rem;
            font-size: 14px;
            list-style: none;
        }
		.BlogAnchor li .nav_item{
			padding: 3px;
		}
        .BlogAnchor li .item_h1{
            margin-left: 0rem;
        }
        .BlogAnchor li .item_h2{
            margin-left: 2rem;
            font-size: 0.8rem;
        }
		.BlogAnchor li .nav_item.current{
			color: white;
			background-color: #5cc26f;
		}
        #AnchorContentToggle {
            font-size: 13px;
            font-weight: normal;
            color: #FFF;
            display: inline-block;
            line-height: 20px;
            background: #5cc26f;
            font-style: normal;
            padding: 1px 8px;
        }
        .BlogAnchor a:hover {
            color: #5cc26f;
        }
        .BlogAnchor a {
            text-decoration: none;
        }
    </style>


在MarkdownPad2中，可以通过菜单->工具->选项->高级->HTML head编辑器来自动插入以上代码。

-----------------------------------
参考：灵感和部分代码来自于[这里](http://www.iyanlei.com/markdown_catelog.html)