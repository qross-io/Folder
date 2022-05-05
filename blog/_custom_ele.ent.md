
```javascript
class HTMLPopupElement1 extends HTMLElement {

    //window,sidebar,menu,alert,confirm,prompt,message
    get type() {
        return this.getAttribute('type');
    }

    set type(type) {
        this.setAttribute('type', type);
    }

    get icon() {
        return this.getAttribute('icon');
    }

    set icon(icon) {
        this.setAttribute('icon', icon);
    }

    get text() {
        return this.getAttribute('text');
    }

    set text(text) {
        this.setAttribute('text', text);
    }

    get position() {
        return this.getAttribute('position');
    }

    set position(position) {
        return this.setAttribute('text', position);
    }

    get offsetX() {
        return $parseFloat(this.getAttribute('offsetx') ?? this.getAttribute('offset-x'), 0);
    }

    set offsetX(offset) {
        this.setAttribute('offset-x', offset);
    }

    get offsetY() {
        return $parseFloat(this.getAttribute('offsety') ?? this.getAttribute('offset-y'), 0);
    }

    set offsetY(offset) {
        this.setAttribute('offset-y', offset);
    }

    get modal() {
        return $parseBoolean(this.getAttribute('modal'), true);
    }

    set modal(modal) {
        this.setAttribute('modal', modal);
    }

    get noScrolling() {
        return $parseBoolean(this.getAttribute('no-scrolling') ?? this.getAttribute('noscrolling'), true);
    }

    set noScrolling(scrolling) {
        this.setAttribute('no-scrolling', scrolling);
    }

    get hidings() {
        if (this._hidings == null) {
            this._hidings = $$(this.getAttribute('hidings'));
        }
        return this._hidings;
    }

    set hidings(hiddings) {
        this._hidings = $$(this.getAttribute('hidings'));
    }

    get maskColor() {
        return this.getAttribute('mask-color') ?? this.getAttribute('maskColor') ?? '#999999';
    }

    //opacity

    show(quick, ev) {
        //如果有其他的popup显示，就隐藏	
        if(document.popup != null) {
            document.popup.hide(true);
        }

        //是否快速显示
        if (!quick) {
            quick = (window.Animation == null || Animation.Entity == null || this.animation == '');
        }

        //隐藏影响popup显示的元素
        this.hidings.forEach(e => e.hide());

        //禁用滚动
        if (this.noScrolling) {
            document.body.style.overflow = 'hidden';
        }    

        //显示Mask
        if(this.modal) {
            PopupMask.show(this.maskColor, this.maskOpacity);
        }

        //显示
        this.element.hidden = false;
        this.locate(ev);
        this.element.style.visibility = 'visible';		
        if(!quick) {
            if (window.Animation != null && Animation.Entity != null) {
                Animation(this.$getOpeningAnimation(ev))
                    .apply(this.element)
                    .play(ev);
            }
        }

        //保存状态
        this.visibility = 'visible';
        document.popup = this;
    }

    hide(quick, ev) {
        if(this.visibility == 'visible') {
            //是否快速隐藏
            if (quick == null) {
                quick = (window.Animation == null || Animation.Entity == null || this.animation == '');
            }
            
            if(!quick) {
                //动画
                if (window.Animation != null && Animation.Entity != null) {
                    let popup = this;
                    Animation(this.$getClosingAnimation(ev))
                        .apply(this.element)
                        .on('stop', function () {
                            popup.element.style.visibility = 'hidden';
                            popup.element.hidden = true;
                            //启用滚动
                            document.body.style.overflow = 'auto';
                        }).play(ev);
                }
            }
            else {		
                //立即隐藏
                this.element.hidden = true;
                this.element.style.visibility = 'hidden';
                                
                //启用滚动
                if (this.disableScrolling) {
                    document.body.style.overflow = 'auto';                
                }
            }
            
            //如果有Mask，隐藏
            if(this.modal) {
                PopupMask.hide(this.maskOpacity);
            }
            
            //显示popup打开时隐藏的元素
            this.hidings.forEach(e => e.show());
    
            document.popup = null;
            this.visibility = 'hidden';
        }
    }

    open() {
        this.initialize();

        if(document.popup == null || document.popup.id != this.id) {
            if(Event.execute(this, 'onopen', ev)) {
                this.show(false, ev);			
                Event.execute(this, 'onshow', ev);
            }
        }
        return this;
    }

    close() {
        if(document.popup == null || document.popup.id == this.id) {
            if(Event.execute(this, 'onclose', ev)) {
                this.hide(false, ev);
                Event.execute(this, 'onhide', ev);
            }
        }
        return this;
    }

    confirm() {
        if(document.popup == null || document.popup.id == this.id) {
            if(Event.execute(this, 'onconfirm', ev)) {
                this.hide(this, false, ev);
                Event.execute(this, 'onhide', ev);
            }
        }
        return this;
    }

    cancel() {
        if(document.popup == null || document.popup.id == this.id) {
            if(Event.execute('oncancel', ev)) {
                this.hide(false, ev);
                Event.execute(this, 'onhide', ev);
            }
        }
        return this;
    }

    initialize() {
        if (!this.initialized) {
            ['onshow', 'onhide', 'onopen', 'onclose', 'onconfirm', 'oncancel']
                .forEach(event => {
                    if (this[event] == null) {
                        this[event] = this.getAttribute(event);
                    }
                });

            Event.interact(this, this);
        }
    }
}
```