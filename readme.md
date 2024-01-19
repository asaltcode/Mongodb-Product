# MangoDb Question and Answers

1. Find all the information about each products.
    * Solution :
      <pre> db.Product.find()</pre>

2. Find the product price which are between 400 to 800.
    * Solution :
      <pre> db.Product.find({product_price:{$gt: 400, $lt: 800}})</pre>

3. Find the product price which are not between 400 to 600.
    * Solution :
      <pre>db.Product.find({product_price:{$not:{$gt: 400, $lt: 600}}})</pre>

4. List the four product which are grater than 500 in price.
    * Solution :
      <pre>db.Product.find({product_price:{$gt: 500}}).limit(4)</pre>

5. Find the product name and product material of each products.
    * Solution :
      <pre> db.Product.find({}, {product_name: 1, product_material: 1})</pre>

6. Find the product with a row id of 10.
    * Solution :
      <pre> db.Product.find({id:"10"}) 
                    (OR)  
       db.Product.find({id:{$eq: '10'}})</pre>

7. Find only the product name and product material.
    * Solution :
      <pre> db.products.find({}, { "product_name": 1, "product_material": 1, "_id": 0 }) 
                                        (OR)
       db.Product.aggregate({$project:{_id:0, product_name: 1, product_material: 1}})</pre>

8. Find all products which contain the value of soft in product material. 
    * Solution :
      <pre> db.Product.find({product_material: 'Soft'})</pre>

9. Find products which contain product color indigo  and product price 492.00.
    * Solution :
      <pre> db.Product.find({$or:[{product_color: 'indigo'}, {product_price: 492}]})</pre>

10. Delete the products which product price value are same.
    * Solution :
      <pre> db.Product.aggregate([
          {
            $group: {_id: "$product_price", count: { $sum: 1 }, ids: { $push: "$_id" }}
          },
          {
             $match: {count: { $gt: 1 }}
          },
          {
            $unwind: "$ids"
          }
        ]).forEach(function(doc){
            db.Product.deleteOne({ids: doc.ids.objectId})
        })
      </pre>
