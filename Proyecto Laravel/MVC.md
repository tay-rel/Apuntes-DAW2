# Ejercicio 5 : Guardar precio 

## Cart.php

```php
 public function getProductPrices($user_id)
	 {
			$sql='SELECT c.product_id, p.price 
			FROM carts c, products p, users u
            WHERE c.user_id=u.id 
            AND c.product_id=p.id 
            AND c.state=0 
            AND user_id=:user_id';
            
			$query = $this->db->prepare($sql);
			$params=[':user_id' => $user_id,];
			$query->execute($params);
			return $query->fetchAll(PDO::FETCH_OBJ);
	 }
	 public function updateProductPrice($product_id, $user_id,$price)
	 {
			$sql='UPDATE carts SET price=:price 
			where state=0 
			AND product_id=:product_id 
			AND user_id=:user_id';
			
			$query = $this->db->prepare($sql);
			$params=[
				':user_id' => $user_id,
				':product_id'=> $product_id,
				':price'=>$price,
			];
			return $query->execute($params);

	 }
```


## CartController.php
```php
public function thanks()
{
        $session = new Session();
        $user = $session->getUser();

		if ($session->getLogin()) {
			//variable para obtener el producto del carrito
			$prices= $this->model->getProductPrices($user->id);

			//foreach recorre prices, que tiene la longitud de lo que tenga carrito
			foreach ($prices as $price)
			{
				$this->model->updateProductPrice(
				$price->product_id,$user->id,$price->price
				);
			}
			if ($this->model->closeCart($user->id, 1)) {

					$data = [
						'titulo' => 'Carrito | Gracias por su compra',
						'data' => $user,
						'menu' => true,
					];
	 
					$this->view('carts/thanks', $data);
					
			} else {
				$data = [
				'titulo' => 'Error en la actualización del carrito',
				
				'menu' => false,
				
				'subtitle' => 'Error en la actualización 
				de los productos del carrito',
				
				'text' => 'Existió un problema al actualizar el estado del carrito. 
				Por favor, pruebe más tarde o comuníquese con nuestro 
				servicio de soporte',
				
				'color' => 'alert-danger',
				
				'url' => 'login',
				
				'colorButton' => 'btn-danger',
				
				'textButton' => 'Regresar',
							 ];
							 
	 
			$this->view('mensaje', $data);
}
```

Para mejor visibilidad en el ejercicio y la base de datos adjunto el repositorio donde eh trabajado este mes:

Guarda precio: 
https://github.com/tay-rel/PhpTiendaMvc/commit/8e2d3c77408102cf9042eb57c62e39fbffadb9a8

Base de datos: 
https://github.com/tay-rel/PhpTiendaMvc/commit/f0e01fe7590438c1cecc9d864f72265f5cb58c39

TiendaMvc:
https://github.com/tay-rel/PhpTiendaMvc
