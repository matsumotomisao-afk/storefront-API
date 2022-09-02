# storefront-API
Shopify storefront APIの使用
<div class="container-fluid d-none d-md-block">
    <div id="top">
        <h2>Storefront API Demo</h2>
      
        <h2>あなたのデバイスに合わせた表示です</h2>
           <h5>全商品から10品目だけを抽出した結果の商品一覧です</h5>
    </div>
<div class="products d-flex d-inline-flex row">
   
      
       
     <div class="yokonarabe">
           <h5>Storefront API による商品の表示デモ</h5>
   <p1>表示されている商品は「https://coolworld.shopの商品を
        私のサイトから呼び出して表示しています。
        これはPC用の表示によって横並びで表示してありますが
        スマホ用の表示を見たい方はこの画面のタブ一覧の右端のウインドウの大きさを変更するアイコンをクリックして、最小のウインドウにして閲覧して下さい。ご覧のように見る人のデバイスによって商品の表示方法が変わるようにプログラムしています。</p1>
        <script>
     
const query = `{
	shop {
		name
	}
}`;

function apiCall(query) {
	return fetch("https://xn-u9jtffy9n7a2539k.myshopify.com/api/2020-10/graphql.json", {
		//[shop-name]にはストアのURLを入力します。
		method: "POST",
		headers: {
			"Content-Type": "application/graphql",
			"X-Shopify-Storefront-Access-Token": "bb2d70a8be570a704c96ac989b5431fe", //Storefront APIのアクセストークンを入力します
		},
		body: query,
	}).then((response) => response.json());
}

apiCall(query).then((response) => {
	console.log(response);
});
function getProducts() {
const query = `{
  products(first:10) {
    edges {
      node {
        title
        description
		onlineStoreUrl
        images(first: 1) {
          edges{
            node {
              altText
              transformedSrc(maxWidth:240, maxHeight: 350)
            }
		
          }
        }
	variants(first: 1) {
          edges {
            node {
              priceV2 {
                amount
              }
            }
          }
        } 
      }
    }
  }
 
}`;
	return apiCall(query);
}
$(document).ready(function () {
	const $app = $("#app");
	getProducts().then((response) => {
		$app.append("<div class='products'></div>");
      	const $products = $(".products");
     response.data.products.edges.forEach((product) => {
                
			const template = `<div class="yokonarabe">
 <div id='img'>
<a href="${product.node.onlineStoreUrl}" + target="_blank" + rel="noopener noreferrer"><img alt="${product.node.images.edges[0].node.altText}"src="${product.node.images.edges[0].node.transformedSrc}"></a></div>
<div id='temp'>
<h5>${product.node.title.substring(0,12) + "..."}</h5>

 ${product.node.description.substring(0,150) + "..."}
<details><summary>read more</summary>${product.node.description.substring(150,)}</details>

PRICE：${product.node.variants.edges[0].node.priceV2.amount + " USD"}
<!--<p>Product-Url</p>
<p>${product.node.onlineStoreUrl}</p><br>-->
<br>
<a href="${product.node.onlineStoreUrl}" + target="_blank" + rel="noopener noreferrer">Show Details</a>
</div>

            </div> `;
			  $products.append(template);
		});
   
	});
   
});
  

        </script>
</div>
</div>
</div>

