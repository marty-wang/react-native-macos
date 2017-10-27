---
id: version-0.45-images
title: images
original_id: images
---
<a id="content"></a><h1><a class="anchor" name="images"></a>Images <a class="hash-link" href="docs/images.html#images">#</a></h1><div><h2><a class="anchor" name="static-image-resources"></a>Static Image Resources <a class="hash-link" href="docs/images.html#static-image-resources">#</a></h2><p>React Native provides a unified way of managing images and other media assets in your iOS and Android apps. To add a static image to your app, place it somewhere in your source code tree and reference it like this:</p><div class="prism language-javascript">&lt;Image source<span class="token operator">=</span><span class="token punctuation">{</span><span class="token function">require<span class="token punctuation">(</span></span><span class="token string">'./my-icon.png'</span><span class="token punctuation">)</span><span class="token punctuation">}</span> <span class="token operator">/</span><span class="token operator">&gt;</span></div><p>The image name is resolved the same way JS modules are resolved. In the example above, the packager will look for <code>my-icon.png</code> in the same folder as the component that requires it. Also, if you have <code>my-icon.ios.png</code> and <code>my-icon.android.png</code>, the packager will pick the correct file for the platform.</p><p>You can also use the <code>@2x</code> and <code>@3x</code> suffixes to provide images for different screen densities. If you have the following file structure:</p><div class="prism language-javascript"><span class="token punctuation">.</span>
├── button<span class="token punctuation">.</span>js
└── img
    ├── check@2x<span class="token punctuation">.</span>png
    └── check@3x<span class="token punctuation">.</span>png</div><p>...and <code>button.js</code> code contains:</p><div class="prism language-javascript">&lt;Image source<span class="token operator">=</span><span class="token punctuation">{</span><span class="token function">require<span class="token punctuation">(</span></span><span class="token string">'./img/check.png'</span><span class="token punctuation">)</span><span class="token punctuation">}</span> <span class="token operator">/</span><span class="token operator">&gt;</span></div><p>...the packager will bundle and serve the image corresponding to device's screen density. For example, <code>check@2x.png</code>, will be used on an iPhone 7, while<code>check@3x.png</code> will be used on an iPhone 7 Plus or a Nexus 5. If there is no image matching the screen density, the closest best option will be selected.</p><p>On Windows, you might need to restart the packager if you add new images to your project.</p><p>Here are some benefits that you get:</p><ol><li>Same system on iOS and Android.</li><li>Images live in the same folder as your JavaScript code. Components are self-contained.</li><li>No global namespace, i.e. you don't have to worry about name collisions.</li><li>Only the images that are actually used will be packaged into your app.</li><li>Adding and changing images doesn't require app recompilation, just refresh the simulator as you normally do.</li><li>The packager knows the image dimensions, no need to duplicate it in the code.</li><li>Images can be distributed via <a href="https://www.npmjs.com/" target="_blank">npm</a> packages.</li></ol><p>In order for this to work, the image name in <code>require</code> has to be known statically.</p><div class="prism language-javascript"><span class="token comment" spellcheck="true">// GOOD
</span>&lt;Image source<span class="token operator">=</span><span class="token punctuation">{</span><span class="token function">require<span class="token punctuation">(</span></span><span class="token string">'./my-icon.png'</span><span class="token punctuation">)</span><span class="token punctuation">}</span> <span class="token operator">/</span><span class="token operator">&gt;</span>
<span class="token comment" spellcheck="true">
// BAD
</span><span class="token keyword">var</span> icon <span class="token operator">=</span> <span class="token keyword">this</span><span class="token punctuation">.</span>props<span class="token punctuation">.</span>active <span class="token operator">?</span> <span class="token string">'my-icon-active'</span> <span class="token punctuation">:</span> <span class="token string">'my-icon-inactive'</span><span class="token punctuation">;</span>
&lt;Image source<span class="token operator">=</span><span class="token punctuation">{</span><span class="token function">require<span class="token punctuation">(</span></span><span class="token string">'./'</span> <span class="token operator">+</span> icon <span class="token operator">+</span> <span class="token string">'.png'</span><span class="token punctuation">)</span><span class="token punctuation">}</span> <span class="token operator">/</span><span class="token operator">&gt;</span>
<span class="token comment" spellcheck="true">
// GOOD
</span><span class="token keyword">var</span> icon <span class="token operator">=</span> <span class="token keyword">this</span><span class="token punctuation">.</span>props<span class="token punctuation">.</span>active <span class="token operator">?</span> <span class="token function">require<span class="token punctuation">(</span></span><span class="token string">'./my-icon-active.png'</span><span class="token punctuation">)</span> <span class="token punctuation">:</span> <span class="token function">require<span class="token punctuation">(</span></span><span class="token string">'./my-icon-inactive.png'</span><span class="token punctuation">)</span><span class="token punctuation">;</span>
&lt;Image source<span class="token operator">=</span><span class="token punctuation">{</span>icon<span class="token punctuation">}</span> <span class="token operator">/</span><span class="token operator">&gt;</span></div><p>Note that image sources required this way include size (width, height) info for the Image. If you need to scale the image dynamically (i.e. via flex), you may need to manually set <code>{ width: undefined, height: undefined }</code> on the style attribute.</p><h2><a class="anchor" name="static-non-image-resources"></a>Static Non-Image Resources <a class="hash-link" href="docs/images.html#static-non-image-resources">#</a></h2><p>The <code>require</code> syntax described above can be used to statically include audio, video or document files in your project as well. Most common file types are supported including <code>.mp3</code>, <code>.wav</code>, <code>.mp4</code>, <code>.mov</code>, <code>.html</code> and <code>.pdf</code> (see the <a href="https://github.com/facebook/react-native/blob/master/packager/defaults.js" target="_blank">packager defaults</a> file for the full list).</p><p>A caveat is that videos must use absolute positioning instead of <code>flexGrow</code>, since size info is not currently passed for non-image assets. This limitation doesn't occur for videos that are linked directly into Xcode or the Assets folder for Android.</p><h2><a class="anchor" name="images-from-hybrid-app-s-resources"></a>Images From Hybrid App's Resources <a class="hash-link" href="docs/images.html#images-from-hybrid-app-s-resources">#</a></h2><p>If you are building a hybrid app (some UIs in React Native, some UIs in platform code) you can still use images that are already bundled into the app (via Xcode asset catalogs or Android drawable folder):</p><div class="prism language-javascript">&lt;Image source<span class="token operator">=</span><span class="token punctuation">{</span><span class="token punctuation">{</span>uri<span class="token punctuation">:</span> <span class="token string">'app_icon'</span><span class="token punctuation">}</span><span class="token punctuation">}</span> style<span class="token operator">=</span><span class="token punctuation">{</span><span class="token punctuation">{</span>width<span class="token punctuation">:</span> <span class="token number">40</span><span class="token punctuation">,</span> height<span class="token punctuation">:</span> <span class="token number">40</span><span class="token punctuation">}</span><span class="token punctuation">}</span> <span class="token operator">/</span><span class="token operator">&gt;</span></div><p>This approach provides no safety checks. It's up to you to guarantee that those images are available in the application. Also you have to specify image dimensions manually.</p><h2><a class="anchor" name="network-images"></a>Network Images <a class="hash-link" href="docs/images.html#network-images">#</a></h2><p>Many of the images you will display in your app will not be available at compile time, or you will want to load some dynamically to keep the binary size down. Unlike with static resources, <em>you will need to manually specify the dimensions of your image</em>. It's highly recommended that you use https as well in order to satisfy <a href="docs/running-on-device.html#app-transport-security" target="_blank">App Transport Security</a> requirements on iOS.</p><div class="prism language-javascript"><span class="token comment" spellcheck="true">// GOOD
</span>&lt;Image source<span class="token operator">=</span><span class="token punctuation">{</span><span class="token punctuation">{</span>uri<span class="token punctuation">:</span> <span class="token string">'https://facebook.github.io/react/img/logo_og.png'</span><span class="token punctuation">}</span><span class="token punctuation">}</span>
       style<span class="token operator">=</span><span class="token punctuation">{</span><span class="token punctuation">{</span>width<span class="token punctuation">:</span> <span class="token number">400</span><span class="token punctuation">,</span> height<span class="token punctuation">:</span> <span class="token number">400</span><span class="token punctuation">}</span><span class="token punctuation">}</span> <span class="token operator">/</span><span class="token operator">&gt;</span>
<span class="token comment" spellcheck="true">
// BAD
</span>&lt;Image source<span class="token operator">=</span><span class="token punctuation">{</span><span class="token punctuation">{</span>uri<span class="token punctuation">:</span> <span class="token string">'https://facebook.github.io/react/img/logo_og.png'</span><span class="token punctuation">}</span><span class="token punctuation">}</span> <span class="token operator">/</span><span class="token operator">&gt;</span></div><h3><a class="anchor" name="network-requests-for-images"></a>Network Requests for Images <a class="hash-link" href="docs/images.html#network-requests-for-images">#</a></h3><p>  If you would like to set such things as the HTTP-Verb, Headers or a Body along with the image request, you may do this by defining these properties on the source object:</p><div class="prism language-javascript">  &lt;Image source<span class="token operator">=</span><span class="token punctuation">{</span><span class="token punctuation">{</span>
      uri<span class="token punctuation">:</span> <span class="token string">'https://facebook.github.io/react/img/logo_og.png'</span><span class="token punctuation">,</span>
      method<span class="token punctuation">:</span> <span class="token string">'POST'</span><span class="token punctuation">,</span>
      headers<span class="token punctuation">:</span> <span class="token punctuation">{</span>
        Pragma<span class="token punctuation">:</span> <span class="token string">'no-cache'</span>
      <span class="token punctuation">}</span><span class="token punctuation">,</span>
      body<span class="token punctuation">:</span> <span class="token string">'Your Body goes here'</span>
    <span class="token punctuation">}</span><span class="token punctuation">}</span>
    style<span class="token operator">=</span><span class="token punctuation">{</span><span class="token punctuation">{</span>width<span class="token punctuation">:</span> <span class="token number">400</span><span class="token punctuation">,</span> height<span class="token punctuation">:</span> <span class="token number">400</span><span class="token punctuation">}</span><span class="token punctuation">}</span> <span class="token operator">/</span><span class="token operator">&gt;</span></div><h3><a class="anchor" name="cache-control-ios-only"></a>Cache Control (iOS Only) <a class="hash-link" href="docs/images.html#cache-control-ios-only">#</a></h3><p>In some cases you might only want to display an image if it is already in the local cache, i.e. a low resolution placeholder until a higher resolution is available. In other cases you do not care if the image is outdated and are willing to display an outdated image to save bandwidth. The <code>cache</code> source property gives you control over how the network layer interacts with the cache.</p><ul><li><code>default</code>: Use the native platforms default strategy.</li><li><code>reload</code>: The data for the URL will be loaded from the originating source.
No existing cache data should be used to satisfy a URL load request.</li><li><code>force-cache</code>: The existing cached data will be used to satisfy the request,
regardless of its age or expiration date. If there is no existing data in the cache
corresponding the request, the data is loaded from the originating source.</li><li><code>only-if-cached</code>: The existing cache data will be used to satisfy a request, regardless of
its age or expiration date. If there is no existing data in the cache corresponding
to a URL load request, no attempt is made to load the data from the originating source,
and the load is considered to have failed.</li></ul><div class="prism language-javascript">&lt;Image source<span class="token operator">=</span><span class="token punctuation">{</span><span class="token punctuation">{</span>uri<span class="token punctuation">:</span> <span class="token string">'https://facebook.github.io/react/img/logo_og.png'</span><span class="token punctuation">,</span> cache<span class="token punctuation">:</span> <span class="token string">'only-if-cached'</span><span class="token punctuation">}</span><span class="token punctuation">}</span>
       style<span class="token operator">=</span><span class="token punctuation">{</span><span class="token punctuation">{</span>width<span class="token punctuation">:</span> <span class="token number">400</span><span class="token punctuation">,</span> height<span class="token punctuation">:</span> <span class="token number">400</span><span class="token punctuation">}</span><span class="token punctuation">}</span> <span class="token operator">/</span><span class="token operator">&gt;</span></div><h2><a class="anchor" name="local-filesystem-images"></a>Local Filesystem Images <a class="hash-link" href="docs/images.html#local-filesystem-images">#</a></h2><p>See <a href="docs/cameraroll.html" target="_blank">CameraRoll</a> for an example of
using local resources that are outside of <code>Images.xcassets</code>.</p><h3><a class="anchor" name="best-camera-roll-image"></a>Best Camera Roll Image <a class="hash-link" href="docs/images.html#best-camera-roll-image">#</a></h3><p>iOS saves multiple sizes for the same image in your Camera Roll, it is very important to pick the one that's as close as possible for performance reasons. You wouldn't want to use the full quality 3264x2448 image as source when displaying a 200x200 thumbnail. If there's an exact match, React Native will pick it, otherwise it's going to use the first one that's at least 50% bigger in order to avoid blur when resizing from a close size. All of this is done by default so you don't have to worry about writing the tedious (and error prone) code to do it yourself.</p><h2><a class="anchor" name="why-not-automatically-size-everything"></a>Why Not Automatically Size Everything? <a class="hash-link" href="docs/images.html#why-not-automatically-size-everything">#</a></h2><p><em>In the browser</em> if you don't give a size to an image, the browser is going to render a 0x0 element, download the image, and then render the image based with the correct size. The big issue with this behavior is that your UI is going to jump all around as images load, this makes for a very bad user experience.</p><p><em>In React Native</em> this behavior is intentionally not implemented. It is more work for the developer to know the dimensions (or aspect ratio) of the remote image in advance, but we believe that it leads to a better user experience. Static images loaded from the app bundle via the <code>require('./my-icon.png')</code> syntax <em>can be automatically sized</em> because their dimensions are available immediately at the time of mounting.</p><p>For example, the result of <code>require('./my-icon.png')</code> might be:</p><div class="prism language-javascript"><span class="token punctuation">{</span><span class="token string">"__packager_asset"</span><span class="token punctuation">:</span><span class="token boolean">true</span><span class="token punctuation">,</span><span class="token string">"uri"</span><span class="token punctuation">:</span><span class="token string">"my-icon.png"</span><span class="token punctuation">,</span><span class="token string">"width"</span><span class="token punctuation">:</span><span class="token number">591</span><span class="token punctuation">,</span><span class="token string">"height"</span><span class="token punctuation">:</span><span class="token number">573</span><span class="token punctuation">}</span></div><h2><a class="anchor" name="source-as-an-object"></a>Source as an object <a class="hash-link" href="docs/images.html#source-as-an-object">#</a></h2><p>In React Native, one interesting decision is that the <code>src</code> attribute is named <code>source</code> and doesn't take a string but an object with a <code>uri</code> attribute.</p><div class="prism language-javascript">&lt;Image source<span class="token operator">=</span><span class="token punctuation">{</span><span class="token punctuation">{</span>uri<span class="token punctuation">:</span> <span class="token string">'something.jpg'</span><span class="token punctuation">}</span><span class="token punctuation">}</span> <span class="token operator">/</span><span class="token operator">&gt;</span></div><p>On the infrastructure side, the reason is that it allows us to attach metadata to this object. For example if you are using <code>require('./my-icon.png')</code>, then we add information about its actual location and size (don't rely on this fact, it might change in the future!). This is also future proofing, for example we may want to support sprites at some point, instead of outputting <code>{uri: ...}</code>, we can output <code>{uri: ..., crop: {left: 10, top: 50, width: 20, height: 40}}</code> and transparently support spriting on all the existing call sites.</p><p>On the user side, this lets you annotate the object with useful attributes such as the dimension of the image in order to compute the size it's going to be displayed in. Feel free to use it as your data structure to store more information about your image.</p><h2><a class="anchor" name="background-image-via-nesting"></a>Background Image via Nesting <a class="hash-link" href="docs/images.html#background-image-via-nesting">#</a></h2><p>A common feature request from developers familiar with the web is <code>background-image</code>. To handle this use case, simply create a normal <code>&lt;Image&gt;</code> component and add whatever children to it you would like to layer on top of it.</p><div class="prism language-javascript"><span class="token keyword">return</span> <span class="token punctuation">(</span>
  &lt;Image source<span class="token operator">=</span><span class="token punctuation">{</span><span class="token punctuation">.</span><span class="token punctuation">.</span><span class="token punctuation">.</span><span class="token punctuation">}</span><span class="token operator">&gt;</span>
    &lt;Text<span class="token operator">&gt;</span>Inside&lt;<span class="token operator">/</span>Text<span class="token operator">&gt;</span>
  &lt;<span class="token operator">/</span>Image<span class="token operator">&gt;</span>
<span class="token punctuation">)</span><span class="token punctuation">;</span></div><h2><a class="anchor" name="ios-border-radius-styles"></a>iOS Border Radius Styles <a class="hash-link" href="docs/images.html#ios-border-radius-styles">#</a></h2><p>Please note that the following corner specific, border radius style properties are currently ignored by iOS's image component:</p><ul><li><code>borderTopLeftRadius</code></li><li><code>borderTopRightRadius</code></li><li><code>borderBottomLeftRadius</code></li><li><code>borderBottomRightRadius</code></li></ul><h2><a class="anchor" name="off-thread-decoding"></a>Off-thread Decoding <a class="hash-link" href="docs/images.html#off-thread-decoding">#</a></h2><p>Image decoding can take more than a frame-worth of time. This is one of the major sources of frame drops on the web because decoding is done in the main thread. In React Native, image decoding is done in a different thread. In practice, you already need to handle the case when the image is not downloaded yet, so displaying the placeholder for a few more frames while it is decoding does not require any code change.</p></div><div class="docs-prevnext"><a class="docs-prev" href="docs/navigation.html#content">← Prev</a><a class="docs-next" href="docs/animations.html#content">Next →</a></div><p class="edit-page-block">You can <a target="_blank" href="https://github.com/facebook/react-native/blob/master/docs/Images.md">edit the content above on GitHub</a> and send us a pull request!</p>