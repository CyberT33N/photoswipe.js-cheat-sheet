# photoswipe.js Cheat Sheet
photoswipe.js Cheat Sheet with the most needed stuff..


<br><br>
<br><br>

# Replace thumbnails with iframe (https://photoswipe.com/custom-content/#google-maps-demo)
```html
<a href="images/loading.svg" data-pswp-width="720" data-pswp-height="400" data-cropped="true" target="_blank" data-pswp-srcset="https://ei.phncdn.com/videos/201908/14/241746531/original/(m=eaf8Ggaaaa)(mh=khS4MN8a95NZ_nTk)13.jpg" data-embed="<iframe></iframe>" data-pswp-type="data-embed"></a>
```

```javascript
// Init lightbox
// children must be the parent element which includes the a tag with href and data-pswp
const lightbox = new PhotoSwipeLightbox({
    gallery: '.swiper-wrapper',
    children: '.swiper-slide',

    showHideAnimationType: 'fade',
    showAnimationDuration: 500,
    hideAnimationDuration: 500,

    pswpModule: () => import('../../photoswipe.esm.js')
})

// parse iframe html
lightbox.addFilter('itemData', (itemData, index) => {
    const outerHTML = itemData.element.outerHTML
    const embed = $(outerHTML).find('a').attr('data-embed')

    if (embed) {
        itemData.embed = embed
    }

    return itemData
})

// override slide content
lightbox.on('contentLoad', e => {
    const { content } = e

    if (content.type === 'data-embed') {
        // prevent the deafult behavior
        e.preventDefault()

        const embed = content.data.embed

        // Create a container for iframe
        // and assign it to the `content.element` property
        content.element = document.createElement('div')
        content.element.className = 'pswp__embed'

        $(content.element).html(embed)
    }
})

lightbox.init()
```
