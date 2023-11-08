<template>
    <component :is="tag" ref="elem" class="scroll-linked">
        <slot></slot>
    </component>
</template>

<script lang="ts">

export interface IScrollLinkedValue {
    x: number;
    y: number;
}

interface IScrollLinkedRect extends IScrollLinkedValue {
    width: number;
    height: number;
}

export class ScrollLinkedValueValidator {
    public static validate(o: any): o is IScrollLinkedValue { 
        return o.x !== undefined && o.y !== undefined;
     }
}

export default {
    data(){
        return {
            element: undefined as HTMLElement | undefined,
            phaseVal: {x:0,y:0} as IScrollLinkedValue,
            viewportFilledVal: {x:0,y:0} as IScrollLinkedValue,
            contentShownVal: {x:0,y:0} as IScrollLinkedValue,
            scrolledInVal: {x:0,y:0} as IScrollLinkedValue,
            scrolledOutVal: {x:0,y:0} as IScrollLinkedValue,
            shownVal: {x:0,y:0} as IScrollLinkedValue,
            shownAlreadyVal: {x:0,y:0} as IScrollLinkedValue,
            sizeVal: {x:0,y:0} as IScrollLinkedValue,
        }
    },
    computed: {
        _phase: {
            get(): IScrollLinkedValue { return this.phaseVal; },
            set(v: IScrollLinkedValue) { this.phaseVal = v;this. $emit("update:phase", v)}
        },
        _viewportFilled: {
            get(): IScrollLinkedValue { return this.viewportFilledVal; },
            set(v: IScrollLinkedValue) { this.viewportFilledVal = v;this. $emit("update:viewportFilled", v)}
        },
        _contentShown: {
            get(): IScrollLinkedValue { return this.contentShownVal; },
            set(v: IScrollLinkedValue) { this.contentShownVal = v;this. $emit("update:contentShown", v)}
        },
        _shown: {
            get(): IScrollLinkedValue { return this.shownVal; },
            set(v: IScrollLinkedValue) { this.shownVal = v;this. $emit("update:shown", v)}
        },
        _scrolledIn: {
            get(): IScrollLinkedValue { return this.scrolledInVal; },
            set(v: IScrollLinkedValue) { this.scrolledInVal = v;this. $emit("update:scrolledIn", v)}
        },
        _scrolledOut: {
            get(): IScrollLinkedValue { return this.scrolledOutVal; },
            set(v: IScrollLinkedValue) { this.scrolledOutVal = v;this. $emit("update:scrolledOut", v)}
        },
        _shownAlready: {
            get(): IScrollLinkedValue { return this.shownAlreadyVal; },
            set(v: IScrollLinkedValue) { this.shownAlreadyVal = v;this. $emit("update:shownAlready", v)}
        },

        _size: {
            get(): IScrollLinkedValue { return this.sizeVal; },
            set(v: IScrollLinkedValue) { this.sizeVal = v;this. $emit("update:size", v)}
        },

    },
    mounted() {
        this.element = this.$refs.elem as HTMLElement;
        document.addEventListener("scroll", () => this.onScroll());
        this.onScroll();
    },
    props: {
        /** Shows how far in the scrolling we are from not yet scrolled in to already scrolled out of view. Values range from 0 to 1 */
        phase: {validator: ScrollLinkedValueValidator.validate},
        /** Shows how much of the viewport is filled with the content. Values range from 0 to 1. May or may not reach 1 */
        viewportFilled: {validator: ScrollLinkedValueValidator.validate},
        /** Shows how much of the content is shown in the Viewport. Values range from 0 to 1. May or may not reach 1 */
        contentShown: {validator: ScrollLinkedValueValidator.validate},
        /** Shows how much of the content is shown. 1 means either all of the content is in the viewport, or completely fills the viewport */
        shown: {validator: ScrollLinkedValueValidator.validate},
        /** Shows how much of the content has been scrolled in. Doesn't show if it's already scrolled out. */
        scrolledIn: {validator: ScrollLinkedValueValidator.validate},
        /** Shows how much of the content has been scrolled out. Doesn't show if it's not yet scrolled in. */
        scrolledOut: {validator: ScrollLinkedValueValidator.validate},
        /** Similar to <ref="shown">shown</ref> but the value doesn't decrease after scrolling past */
        shownAlready: {validator: ScrollLinkedValueValidator.validate},
        /** content size */
        size: {validator: ScrollLinkedValueValidator.validate},
        
        tag: {type: String, default: "div"}
    },
    methods: {
        onScroll() {
            const rect = this.element?.getBoundingClientRect();

            if (rect === undefined)
                return;

            const vRect = {
                x: 0,
                y: 0,
                width: window.innerWidth || document.documentElement.clientWidth,
                height: window.innerHeight || document.documentElement.clientHeight,
            };

            this._size = { x: rect.width, y: rect.height };
            this.getPhase(rect, vRect);
            this.getViewportFilled(rect, vRect);
            this.getContentShown(rect, vRect);
            this.getScrolledIn(rect, vRect);
            this.getScrolledOut(rect, vRect);
            this.getShown(rect, vRect);
        },
        getPhase(bounds: IScrollLinkedRect, client:IScrollLinkedRect) {
            if(this.phase === undefined)
                return;
            let x = (bounds.x + bounds.width) / (bounds.width + client.width);
            let y = (bounds.y + bounds.height) / (bounds.height + client.height);
            this._phase = this.clamp(1-x,1-y);
        },
        getViewportFilled(bounds: IScrollLinkedRect, client:IScrollLinkedRect) {
            if(this.viewportFilled === undefined)
                return;

            let x = Math.min(bounds.x + bounds.width, client.x + client.width) - Math.max(bounds.x, client.x);
            let y = Math.min(bounds.y + bounds.height, client.y + client.height) - Math.max(bounds.y, client.y);

            x /= client.width;
            y /= client.height;

            this._viewportFilled = this.clamp(x,y);
        },
        getContentShown(bounds: IScrollLinkedRect, client:IScrollLinkedRect) {
            if(this.contentShown === undefined)
                return;

            let x = Math.min(bounds.x + bounds.width, client.x + client.width) - Math.max(bounds.x, client.x);
            let y = Math.min(bounds.y + bounds.height, client.y + client.height) - Math.max(bounds.y, client.y);

            x /= bounds.width;
            y /= bounds.height;

            this._contentShown = this.clamp(x,y);
        },
        getScrolledIn(bounds: IScrollLinkedRect, client:IScrollLinkedRect) {
            if(this.scrolledIn === undefined)
                return;

            let x = Math.min(bounds.x, client.width);
            let y = Math.min(bounds.y, client.height);

            x = 1 - Math.max(x, 0) / client.width;
            y = 1 - Math.max(y, 0) / client.height;

            this._scrolledIn = this.clamp(x,y);
        },
        getScrolledOut(bounds: IScrollLinkedRect, client:IScrollLinkedRect) {
            if(this.scrolledOut === undefined)
                return;

            let x = Math.min(bounds.x + bounds.width, client.width);
            let y = Math.min(bounds.y + bounds.height, client.height);

            x = 1 - Math.max(x, 0) / client.width;
            y = 1 - Math.max(y, 0) / client.height;

            this._scrolledOut =  this.clamp(x,y);
        },
        getShown(bounds: IScrollLinkedRect, client:IScrollLinkedRect) {
            if(this.shown === undefined && this.shownAlready === undefined)
                return;

            let x = Math.min(bounds.x + bounds.width, client.x + client.width) - Math.max(bounds.x, client.x);
            let y = Math.min(bounds.y + bounds.height, client.y + client.height) - Math.max(bounds.y, client.y);

            x /= Math.min(bounds.width, client.width);
            y /= Math.min(bounds.height, client.height);

            this._shown = this.clamp(x,y);

            if(bounds.x < client.x || bounds.x + bounds.width < client.x + client.width) x = 1;
            if(bounds.y < client.y || bounds.y + bounds.height < client.y + client.height) y = 1;
            this._shownAlready = this.clamp(x,y); 
        },
        clamp(x: number, y: number) : IScrollLinkedValue {
            x = x > 1 ? 1 : x < 0 ? 0 : x;
            y = y > 1 ? 1 : y < 0 ? 0 : y;
            return {x, y};
        }
    }
}
</script>