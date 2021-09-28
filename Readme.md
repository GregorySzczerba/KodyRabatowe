# Kody rabatowe dla klientów (Prestashop)
Moduł pozwala wygenerować masowo kody rabatowe w Prestashop.
W przyszłości dodana zostanie opcja konfiguracji parametrów tych kodów we frontendzie panelu administracyjnego prestashop.
W tej chwili te opcje możemy zmienić w kodzie, funkcja:
 protected function generateVouchers() // w pliku KodyRabatowe.php
    {
		
	for($i = 1; $i <= 50; $i++) //możemy ustawić ilość kodów poprzez zmianę ilości wykonania pętli, w tej chwili 50 razy
		{	
		$now = time(); 
		$code = 'ALL_'.strtoupper(substr(base_convert(sha1(uniqid(mt_rand())), 16, 36), 0, 10));
		$cart_rule = new CartRule();
		$cart_rule->code = $code;
		$cart_rule->name[1] = ('allegro'. '-' . $code); // tu możemy dodać prefiks do każdego kodu
		$cart_rule->description = $this->trans('Kod rabatowy dla klientów z allegro');
		$cart_rule->date_from = date('Y-m-d H:i:s', $now); // data od kiedy kod ma obowiązywać, od chwili wygenerowanie
		$cart_rule->date_to = date('Y-m-d H:i:s', strtotime('+2 year')); // przez 2 lata
		$cart_rule->partial_use = false; // częściowe użycie niemożliwe
		$cart_rule->quantity = 1; // ilość -  1 sztuka
		$cart_rule->quantity_per_user = 1; // ilość na użytkownika
		$cart_rule->reduction_percent = 10; // skala upustu 10 %
		$cart_rule->free_shipping = false; // kod nie obejmuje darmowej wysyłki
		
		$cart_rule->cart_rule_restriction = 1;
		$cart_rule->add();
		}
	}
