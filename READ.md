<h1> API de tasa de cambio </h1>
<h2>Hilario SalgadoAntonio</h2>

Crear las clases de modelo:
paquete com.example.exchangeapi.model;

importar javax.persistence.Entity;
importar javax.persistence.GeneratedValue;
importar javax.persistence.GenerationType;
importar javax.persistence.Id;

@Entidad
clase pública ExchangeRate {
    @Identificación
    @GeneratedValue(estrategia = TipoGeneración.IDENTIDAD)
    privado id largo;

    Cadena privada de Moneda;
    Cadena privada a Moneda;
    tarifa doble privada;

    // Captadores y definidores
}

Crear el repositorio:
paquete com.example.exchangeapi.repository;

importar com.example.exchangeapi.model.ExchangeRate;
importar org.springframework.data.jpa.repository.JpaRepository;

importar java.util.Opcional;

Interfaz pública ExchangeRateRepository extiende JpaRepository<ExchangeRate, Long> {
    Opcional<ExchangeRate> findByFromCurrencyAndToCurrency(Cadena desdeMoneda, Cadena aMoneda);
}

Crear el servicio:
paquete com.example.exchangeapi.service;

importar com.example.exchangeapi.model.ExchangeRate;
importar com.example.exchangeapi.repository.ExchangeRateRepository;
importar org.springframework.beans.factory.annotation.Autowired;
importar org.springframework.stereotype.Service;

importar java.util.Opcional;

@Servicio
clase pública ExchangeRateService {
    @Autowired
    ExchangeRateRepository privado exchangeRateRepository;

    público Opcional<ExchangeRate> obtenerExchangeRate(Cadena deMoneda, Cadena aMoneda) {
        devuelve exchangeRateRepository.findByFromCurrencyAndToCurrency(desdeCurrency, hastaCurrency);
    }
}

Crear el controlador:
paquete com.example.exchangeapi.controller;

importar com.example.exchangeapi.model.ExchangeRate;
importar com.example.exchangeapi.service.ExchangeRateService;
importar org.springframework.beans.factory.annotation.Autowired;
importar org.springframework.http.ResponseEntity;
importar org.springframework.web.bind.annotation.GetMapping;
importar org.springframework.web.bind.annotation.RequestParam;
importar org.springframework.web.bind.annotation.RestController;

importar java.util.Opcional;

@ControladorRest
clase pública ExchangeRateController {
    @Autowired
    servicio de tipo de cambio privado servicio de tipo de cambio privado;

    @GetMapping("/tipo de cambio")
    pública ResponseEntity<ExchangeRate> getExchangeRate(
            @RequestParam Cadena de Moneda,
            @RequestParam Cadena a Moneda) {
        Opcional<ExchangeRate> exchangeRate = exchangeRateService.getExchangeRate(fromCurrency, toCurrency);
        devuelve exchangeRate.map(ResponseEntity::ok)
                .orElseGet(() -> ResponseEntity.notFound().build());
    }
}
