<div class="col-checkout">
			<div>
	
				<h3>Оплата и доставка</h3>
				<form class="review-form" @submit.prevent="onSubmit">
					<p class="name">
						<label for="name">Имя:</label>
						<input id="name" v-model="name">
					</p>
					<p class="last_name">
						<label for="last_name">Фамилия:</label>
						<input id="last_name" v-model="last_name">
					</p>
					<p class="tel">
						<label for="tel">Телефон:</label>
						<input id="tel" v-model="tel">
					</p>
					<p class="mail">
						<label for="mail">Телефон:</label>
						<input id="mail" v-model="mail">
					</p>

					<p>
						<input type="submit" value="Submit">  
					</p>

				</form>

			</div>
		</div>