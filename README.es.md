# ClinicaCore 🏥

Plataforma integral de gestión clínica construida sobre **arquitectura de microservicios**,
diseñada para digitalizar y optimizar las operaciones de clínicas y consultorios médicos.
Integra gestión avanzada de historias clínicas, agendamiento inteligente de citas,
facturación electrónica compatible con **DIAN** para servicios de salud, y
**capacidades de búsqueda de alto rendimiento impulsadas por ElasticSearch** sobre todos los datos clínicos.

Desarrollado para clínicas privadas y consultorios médicos colombianos que requieren cumplimiento con
la **Resolución 1995/1999** (historia clínica electrónica), **reportes RIPS** para entidades EPS,
y regulaciones de facturación electrónica DIAN.

---

## 🛠️ Stack Tecnológico Completo

### Backend — Microservicios

| Tecnología               | Versión   | Propósito                                          |
|--------------------------|-----------|----------------------------------------------------|
| **Java**                 | 11        | Lenguaje principal del backend                     |
| **Spring Boot**          | 2.7.x     | Framework de microservicios (compatible Java 11)   |
| **Spring MVC**           | 5.3.x     | Controladores REST y manejo de peticiones          |
| **Spring Cloud**         | 2021.x    | Soporte para sistemas distribuidos                 |
| **Spring Cloud Gateway** | —         | API Gateway con limitación de velocidad            |
| **Eureka Server**        | —         | Descubrimiento de servicios                        |
| **Spring Cloud Config**  | —         | Gestión centralizada de configuración              |
| **Spring Data JPA**      | —         | ORM y persistencia de datos                        |
| **Spring Security**      | 5.7.x     | Autenticación y autorización                       |
| **Resilience4j**         | 1.7.x     | Circuit Breaker y limitación de velocidad          |
| **Apache Kafka**         | 3.x       | Mensajería asíncrona orientada a eventos           |
| **ElasticSearch**        | 7.17.x    | Búsqueda de texto completo e indexación clínica    |
| **Logstash**             | 7.17.x    | Pipeline de datos hacia ElasticSearch              |
| **Kibana**               | 7.17.x    | Analítica de búsqueda y visualización de logs      |
| **PostgreSQL**           | 15+       | Base de datos relacional principal                 |
| **MongoDB**              | 6.x       | Logs clínicos y documentos no estructurados        |
| **Redis**                | 7.x       | Caché distribuida y gestión de sesiones            |

### Frontend — Single Page Application

| Tecnología                  | Versión | Propósito                                          |
|-----------------------------|---------|----------------------------------------------------|
| **React**                   | 18.x    | Framework de UI para el frontend                   |
| **TypeScript**              | 5.x     | Tipado estático y POO                              |
| **React-Redux**             | 8.x     | Gestión de estado global                           |
| **Redux Toolkit**           | 1.9.x   | Redux simplificado con slices                      |
| **Redux Thunk**             | 2.4.x   | Middleware asíncrono para llamadas a la API        |
| **React Router v6**         | 6.x     | Enrutamiento del lado del cliente                  |
| **Webpack**                 | 5.x     | Empaquetado manual de módulos y optimización       |
| **SASS/SCSS**               | 1.x     | Preprocesamiento avanzado de CSS                   |
| **Axios**                   | 1.x     | Cliente HTTP con interceptores                     |
| **React Testing Library**   | 14.x    | Pruebas unitarias de componentes (TDD)             |
| **Cypress**                 | 13.x    | Pruebas end-to-end (BDD)                           |
| **Chart.js**                | 4.x     | Dashboards clínicos y gráficas analíticas          |
| **Socket.io Client**        | 4.x     | Actualizaciones en tiempo real por WebSocket       |

### Integraciones Externas

| Sistema                   | Propósito                                                  |
|---------------------------|------------------------------------------------------------|
| **DIAN Web Services**     | Facturación electrónica oficial para servicios de salud    |
| **DIAN RADIAN**           | Gestión de eventos de factura electrónica                  |
| **RIPS (MinSalud)**       | Reporte obligatorio de prestaciones para EPS               |
| **ADRES API**             | Validación de beneficiarios y cobertura                    |
| **PayU / Mercado Pago**   | Integración con pasarelas de pago                          |

### DevOps e Infraestructura

| Tecnología          | Propósito                                                        |
|---------------------|------------------------------------------------------------------|
| **Docker**          | Contenedorización de servicios                                   |
| **Docker Compose**  | Orquestación local de múltiples servicios                        |
| **Kubernetes**      | Orquestación de contenedores para producción                     |
| **Jenkins**         | Automatización de pipelines CI/CD                                |
| **Prometheus**      | Recolección de métricas y alertas                                |
| **Grafana**         | Monitoreo de infraestructura y aplicaciones                      |
| **ELK Stack**       | Logging centralizado (ElasticSearch + Logstash + Kibana)         |

---

## 🏗️ Arquitectura de Microservicios
```
                    ┌─────────────────────────────────────────┐
                    │         React 18 Frontend SPA           │
                    │  (Redux + Thunks · SASS · Webpack 5)   │
                    └─────────────────────────────────────────┘
                                        ↓
                    ┌─────────────────────────────────────────┐
                    │     API Gateway (Spring Cloud Gateway)  │
                    │  - Validación JWT                       │
                    │  - Limitación de Velocidad (Resilience4j│
                    │  - Balanceo de Carga                    │
                    │  - Filtrado de Rutas                    │
                    └─────────────────────────────────────────┘
                                        ↓
       ┌──────────────┬─────────────────┬──────────────┬─────────────┬──────────────┐
       ↓              ↓                 ↓              ↓             ↓              ↓
┌───────────┐  ┌────────────┐  ┌──────────────┐  ┌──────────┐ ┌──────────┐ ┌───────────┐
│  Servicio │  │  Servicio  │  │   Servicio   │  │ Servicio │ │ Servicio │ │  Servicio │
│ Pacientes │  │   Citas    │  │ Hist.Clínica │  │Facturac. │ │ Búsqueda │ │   RIPS    │
│           │  │            │  │              │  │          │ │          │ │           │
│ - CRUD    │  │ - Agenda   │  │ - Gestión HCE│  │ - DIAN   │ │ - ES     │ │ - MinSal  │
│ - Historial│  │ - Calend. │  │ - Diagnóst.  │  │ - PDF    │ │   Índice │ │   Reporte │
│ - Consent.│  │ - Recuerd. │  │ - Tratamient.│  │ - PayU   │ │ - Consul.│ │ - EPS     │
└───────────┘  └────────────┘  └──────────────┘  └──────────┘ └──────────┘ └───────────┘
       ↓              ↓                 ↓              ↓             ↓              ↓
┌───────────┐  ┌────────────┐  ┌──────────────┐  ┌──────────┐ ┌──────────┐ ┌───────────┐
│PostgreSQL │  │ PostgreSQL │  │  PostgreSQL  │  │PostgreSQL│ │ElasticSe-│ │  MongoDB  │
│(Pacientes)│  │  (Citas)   │  │  (Historias) │  │(Factura.)│ │  arch    │ │  (Logs)   │
└───────────┘  └────────────┘  └──────────────┘  └──────────┘ └──────────┘ └───────────┘

                    ┌─────────────────────────────────────────┐
                    │       Bus de Mensajes Apache Kafka      │
                    │  Tópicos:                               │
                    │  - patient.registered                   │
                    │  - appointment.confirmed                │
                    │  - record.updated                       │
                    │  - billing.issued                       │
                    │  - search.index.requested               │
                    │  - rips.report.generated                │
                    └─────────────────────────────────────────┘
                                        ↓
          ┌─────────────────────────────────────────────────────┐
          │            ELK Stack — Observabilidad               │
          │   ElasticSearch · Logstash · Kibana                 │
          │   (Logs de auditoría clínica + analítica búsqueda)  │
          └─────────────────────────────────────────────────────┘
                                        ↓
                    ┌─────────────────────────────────────────┐
                    │      Servicio de Integración DIAN       │
                    │  - Firma Digital (PKCS#12)              │
                    │  - Generación XML UBL 2.1               │
                    │  - Cálculo CUFE                         │
                    │  - Validación DIAN en Tiempo Real       │
                    └─────────────────────────────────────────┘
                                        ↓
                    ┌─────────────────────────────────────────┐
                    │         Servicios Web DIAN              │
                    │   (Facturación Electrónica - Salud)     │
                    └─────────────────────────────────────────┘
```

---

## ⚙️ Responsabilidades de cada Servicio

### Servicio de Pacientes
Servicio principal que gestiona todas las operaciones del ciclo de vida del paciente.
Maneja el registro de pacientes con datos demográficos completos, gestión del
consentimiento bajo la Ley 1581 de Colombia (protección de datos), versionado de
historia clínica, y publica eventos en Kafka ante cualquier cambio de estado del
paciente para que los servicios dependientes (Búsqueda, RIPS) se mantengan sincronizados.

### Servicio de Citas
Gestiona el ciclo de vida completo del agendamiento: disponibilidad de slots por
médico y especialidad, reserva con detección de conflictos, recordatorios automáticos
por correo y SMS, y flujos de cancelación. Publica eventos `appointment.confirmed`
que consume el Servicio de Historia Clínica para pre-crear entradas de consulta.

### Servicio de Historia Clínica Electrónica (HCE)
El servicio más sensible de la arquitectura. Almacena y versiona las historias clínicas
electrónicas en cumplimiento con la **Resolución 1995/1999**. Cada mutación del registro
es inmutable — las actualizaciones generan nuevas versiones, nunca sobrescriben.
Los diagnósticos siguen la codificación CIE-10, y los tratamientos están vinculados
a prescripciones con trazabilidad completa.

### Servicio de Facturación
Maneja la facturación electrónica para servicios médicos con integración completa DIAN.
Genera XML compatible con UBL 2.1, calcula el CUFE, gestiona la firma digital, y procesa
los callbacks de las pasarelas de pago PayU y Mercado Pago. Publica eventos que disparan
asientos contables automáticos en servicios posteriores.

### Servicio de Búsqueda (Núcleo ElasticSearch)
El servicio diferenciador de esta arquitectura. Consume eventos de todos los demás
servicios a través de Kafka y mantiene un índice ElasticSearch desnormalizado y
optimizado. Expone endpoints de búsqueda avanzada: búsqueda de texto completo de
pacientes, consulta de códigos CIE-10, búsqueda de medicamentos con autocompletado,
y consultas analíticas clínicas entre servicios. Implementa gestión del ciclo de vida
de índices (ILM) para políticas de retención de logs de auditoría.

### Servicio RIPS
Genera los reportes RIPS (Registro Individual de Prestación de Servicios) obligatorios
exigidos por el Ministerio de Salud de Colombia (MinSalud) para la facturación a EPS.
Agrega datos de los servicios de Pacientes, Citas e Historia Clínica y produce el
formato de salida estandarizado para la presentación regulatoria.

---

## ✨ Funcionalidades Principales

### 🔍 ElasticSearch — Búsqueda Clínica Avanzada

ElasticSearch es el diferenciador principal de ClinicaCore. Cada servicio publica
eventos en Kafka, que son consumidos por el Servicio de Búsqueda para mantener un
índice desnormalizado en tiempo real, optimizado para consultas clínicas. Esto permite
búsqueda de texto completo en menos de un segundo sobre pacientes, diagnósticos,
tratamientos e historias clínicas de forma simultánea.

**Configuración del Índice — Índice Clínico de Pacientes**
```java
// Servicio de Búsqueda — Configuración del Índice ElasticSearch
// Java 11 + Spring Boot 2.7.x + Spring Data ElasticSearch 4.4.x

@Configuration
public class ElasticSearchConfig extends AbstractElasticsearchConfiguration {

    @Value("${elasticsearch.host}")
    private String host;

    @Value("${elasticsearch.port}")
    private int port;

    @Override
    @Bean
    public RestHighLevelClient elasticsearchClient() {
        ClientConfiguration clientConfiguration = ClientConfiguration.builder()
            .connectedTo(host + ":" + port)
            .withConnectTimeout(Duration.ofSeconds(5))
            .withSocketTimeout(Duration.ofSeconds(30))
            .withBasicAuth("${elasticsearch.user}", "${elasticsearch.password}")
            .build();

        return RestClients.create(clientConfiguration).rest();
    }
}
```
```java
// Documento Clínico de Paciente — Mapeo del Índice ElasticSearch
@Document(indexName = "patients", createIndex = true)
@Setting(settingPath = "elasticsearch/patient-settings.json")
@Mapping(mappingPath = "elasticsearch/patient-mapping.json")
public class PatientDocument {

    @Id
    private String id;

    @Field(type = FieldType.Text, analyzer = "spanish")
    private String fullName;

    @Field(type = FieldType.Keyword)
    private String documentNumber;

    @Field(type = FieldType.Text, analyzer = "spanish")
    private String address;

    @Field(type = FieldType.Keyword)
    private String bloodType;

    @Field(type = FieldType.Nested)
    private List<DiagnosisDocument> diagnoses;

    @Field(type = FieldType.Nested)
    private List<MedicationDocument> medications;

    @Field(type = FieldType.Text, analyzer = "spanish")
    private String clinicalNotes;

    @Field(type = FieldType.Date, format = DateFormat.date)
    private LocalDate lastVisit;

    @Field(type = FieldType.Keyword)
    private String epsCode;

    @Field(type = FieldType.Boolean)
    private boolean active;
}
```
```java
// Servicio de Búsqueda — Motor de Consultas Clínicas
@Service
@Slf4j
public class ClinicalSearchService {

    private final ElasticsearchOperations elasticsearchOperations;
    private final RestHighLevelClient restHighLevelClient;

    public ClinicalSearchService(ElasticsearchOperations elasticsearchOperations,
                                  RestHighLevelClient restHighLevelClient) {
        this.elasticsearchOperations = elasticsearchOperations;
        this.restHighLevelClient = restHighLevelClient;
    }

    // Búsqueda de texto completo multi-campo de pacientes
    public SearchHits<PatientDocument> searchPatients(ClinicalSearchRequest request) {
        NativeSearchQueryBuilder queryBuilder = new NativeSearchQueryBuilder();

        // Multi-match sobre nombre, documento y notas clínicas
        MultiMatchQueryBuilder multiMatch = QueryBuilders.multiMatchQuery(
                request.getTerm(),
                "fullName^3",          // mayor relevancia al nombre
                "documentNumber^5",    // máxima prioridad
                "clinicalNotes",
                "diagnoses.description"
        ).type(MultiMatchQueryBuilder.Type.BEST_FIELDS)
         .fuzziness(Fuzziness.AUTO);   // tolerancia a errores tipográficos

        BoolQueryBuilder boolQuery = QueryBuilders.boolQuery()
            .must(multiMatch)
            .filter(QueryBuilders.termQuery("active", true));

        // Filtro por EPS cuando se proporciona
        if (request.getEpsCode() != null) {
            boolQuery.filter(QueryBuilders.termQuery("epsCode", request.getEpsCode()));
        }

        // Filtro por rango de fecha de última visita
        if (request.getFromDate() != null) {
            boolQuery.filter(QueryBuilders.rangeQuery("lastVisit")
                .gte(request.getFromDate())
                .lte(request.getToDate() != null ? request.getToDate() : LocalDate.now()));
        }

        NativeSearchQuery searchQuery = queryBuilder
            .withQuery(boolQuery)
            .withHighlightFields(
                new HighlightBuilder.Field("fullName"),
                new HighlightBuilder.Field("clinicalNotes")
            )
            .withPageable(PageRequest.of(request.getPage(), request.getSize()))
            .withSort(SortBuilders.scoreSort().order(SortOrder.DESC))
            .build();

        log.info("Ejecutando búsqueda clínica: term={}, eps={}", 
            request.getTerm(), request.getEpsCode());

        return elasticsearchOperations.search(searchQuery, PatientDocument.class);
    }

    // Autocompletado de diagnósticos — CIE-10
    public List<String> autocompleteDiagnosis(String prefix) {
        CompletionSuggestionBuilder suggestionBuilder =
            SuggestBuilders.completionSuggestion("diagnoses.suggest")
                .prefix(prefix)
                .size(10)
                .skipDuplicates(true);

        SearchSourceBuilder sourceBuilder = new SearchSourceBuilder()
            .suggest(new SuggestBuilder()
                .addSuggestion("diagnosis-suggest", suggestionBuilder));

        SearchRequest searchRequest = new SearchRequest("patients")
            .source(sourceBuilder);

        try {
            SearchResponse response = restHighLevelClient.search(
                searchRequest, RequestOptions.DEFAULT);

            return response.getSuggest()
                .getSuggestion("diagnosis-suggest")
                .getEntries()
                .stream()
                .flatMap(entry -> entry.getOptions().stream())
                .map(option -> option.getText().string())
                .collect(Collectors.toList());

        } catch (IOException e) {
            log.error("Error de autocompletado en ElasticSearch", e);
            throw new SearchServiceException("Autocompletado fallido", e);
        }
    }

    // Indexar paciente desde evento Kafka
    @KafkaListener(topics = "search.index.requested", groupId = "search-service")
    public void indexPatientEvent(PatientIndexEvent event) {
        PatientDocument document = PatientDocument.builder()
            .id(event.getPatientId().toString())
            .fullName(event.getFullName())
            .documentNumber(event.getDocumentNumber())
            .diagnoses(mapDiagnoses(event.getDiagnoses()))
            .medications(mapMedications(event.getMedications()))
            .clinicalNotes(event.getLatestNotes())
            .lastVisit(event.getLastVisit())
            .epsCode(event.getEpsCode())
            .active(true)
            .build();

        elasticsearchOperations.save(document);
        log.info("Paciente indexado en ElasticSearch: id={}", event.getPatientId());
    }
}
```

---

### ⚛️ React — Redux Toolkit + Thunks

El frontend sigue una **arquitectura Redux estricta** con slices de Redux Toolkit y
Redux Thunk para todas las operaciones asíncronas. Webpack 5 está configurado
manualmente para control total sobre la división de código, optimización de bundles
y compilación de SASS — sin abstracciones de Create React App.

**Configuración del Store Redux**
```typescript
// store/index.ts — Store Redux con middleware Thunk
import { configureStore } from '@reduxjs/toolkit';
import patientReducer from './slices/patientSlice';
import searchReducer from './slices/searchSlice';
import appointmentReducer from './slices/appointmentSlice';
import authReducer from './slices/authSlice';
import { apiMiddleware } from './middleware/apiMiddleware';

export const store = configureStore({
  reducer: {
    patients: patientReducer,
    search: searchReducer,
    appointments: appointmentReducer,
    auth: authReducer,
  },
  middleware: (getDefaultMiddleware) =>
    getDefaultMiddleware({
      serializableCheck: {
        ignoredActions: ['persist/PERSIST'],
      },
    }).concat(apiMiddleware),
  devTools: process.env.NODE_ENV !== 'production',
});

export type RootState = ReturnType<typeof store.getState>;
export type AppDispatch = typeof store.dispatch;
```
```typescript
// store/slices/searchSlice.ts — Integración Redux con ElasticSearch
import { createSlice, createAsyncThunk, PayloadAction } from '@reduxjs/toolkit';
import { searchService } from '../../services/searchService';
import { PatientSearchResult, ClinicalSearchParams } from '../../types/search';

interface SearchState {
  results: PatientSearchResult[];
  suggestions: string[];
  loading: boolean;
  error: string | null;
  totalHits: number;
  currentPage: number;
  query: string;
}

const initialState: SearchState = {
  results: [],
  suggestions: [],
  loading: false,
  error: null,
  totalHits: 0,
  currentPage: 0,
  query: '',
};

// Async Thunk — Búsqueda clínica de texto completo
export const searchPatients = createAsyncThunk(
  'search/searchPatients',
  async (params: ClinicalSearchParams, { rejectWithValue }) => {
    try {
      const response = await searchService.searchPatients(params);
      return response.data;
    } catch (error: any) {
      return rejectWithValue(error.response?.data?.message || 'Búsqueda fallida');
    }
  }
);

// Async Thunk — Autocompletado de diagnósticos CIE-10
export const fetchDiagnosisSuggestions = createAsyncThunk(
  'search/fetchSuggestions',
  async (prefix: string, { rejectWithValue }) => {
    try {
      const response = await searchService.autocompleteDiagnosis(prefix);
      return response.data;
    } catch (error: any) {
      return rejectWithValue(error.response?.data?.message || 'Autocompletado fallido');
    }
  }
);

const searchSlice = createSlice({
  name: 'search',
  initialState,
  reducers: {
    setQuery: (state, action: PayloadAction<string>) => {
      state.query = action.payload;
    },
    clearResults: (state) => {
      state.results = [];
      state.totalHits = 0;
      state.currentPage = 0;
    },
    setPage: (state, action: PayloadAction<number>) => {
      state.currentPage = action.payload;
    },
  },
  extraReducers: (builder) => {
    builder
      .addCase(searchPatients.pending, (state) => {
        state.loading = true;
        state.error = null;
      })
      .addCase(searchPatients.fulfilled, (state, action) => {
        state.loading = false;
        state.results = action.payload.hits;
        state.totalHits = action.payload.totalHits;
      })
      .addCase(searchPatients.rejected, (state, action) => {
        state.loading = false;
        state.error = action.payload as string;
      })
      .addCase(fetchDiagnosisSuggestions.fulfilled, (state, action) => {
        state.suggestions = action.payload;
      });
  },
});

export const { setQuery, clearResults, setPage } = searchSlice.actions;
export default searchSlice.reducer;
```
```typescript
// components/ClinicalSearch/ClinicalSearch.tsx
import React, { useCallback, useEffect, useRef } from 'react';
import { useDispatch, useSelector } from 'react-redux';
import { AppDispatch, RootState } from '../../store';
import {
  searchPatients,
  fetchDiagnosisSuggestions,
  setQuery,
  clearResults,
} from '../../store/slices/searchSlice';
import styles from './ClinicalSearch.module.scss';

const ClinicalSearch: React.FC = () => {
  const dispatch = useDispatch<AppDispatch>();
  const { results, suggestions, loading, error, totalHits, query } =
    useSelector((state: RootState) => state.search);

  const debounceRef = useRef<NodeJS.Timeout>();

  const handleQueryChange = useCallback(
    (e: React.ChangeEvent<HTMLInputElement>) => {
      const value = e.target.value;
      dispatch(setQuery(value));

      // Autocompletado con debounce mediante Thunk
      clearTimeout(debounceRef.current);
      debounceRef.current = setTimeout(() => {
        if (value.length >= 3) {
          dispatch(fetchDiagnosisSuggestions(value));
        }
      }, 300);
    },
    [dispatch]
  );

  const handleSearch = useCallback(() => {
    if (query.trim().length < 2) return;
    dispatch(searchPatients({ term: query, page: 0, size: 20 }));
  }, [dispatch, query]);

  useEffect(() => {
    return () => {
      dispatch(clearResults());
    };
  }, [dispatch]);

  return (
    <div className={styles.searchContainer}>
      <div className={styles.searchBar}>
        <input
          type="text"
          value={query}
          onChange={handleQueryChange}
          placeholder="Buscar pacientes, diagnósticos, medicamentos..."
          className={styles.searchInput}
          data-testid="clinical-search-input"
        />
        <button
          onClick={handleSearch}
          disabled={loading}
          className={styles.searchButton}
          data-testid="search-button"
        >
          {loading ? 'Buscando...' : 'Buscar'}
        </button>
      </div>

      {suggestions.length > 0 && (
        <ul className={styles.suggestionList} data-testid="suggestions-list">
          {suggestions.map((suggestion, index) => (
            <li
              key={index}
              className={styles.suggestionItem}
              onClick={() => dispatch(setQuery(suggestion))}
            >
              {suggestion}
            </li>
          ))}
        </ul>
      )}

      {error && (
        <div className={styles.errorMessage} data-testid="search-error">
          {error}
        </div>
      )}

      <div className={styles.resultsHeader}>
        {totalHits > 0 && (
          <span>{totalHits} registros clínicos encontrados</span>
        )}
      </div>

      <div className={styles.resultsList}>
        {results.map((patient) => (
          <div
            key={patient.id}
            className={styles.resultCard}
            data-testid="patient-result-card"
          >
            <h3>{patient.fullName}</h3>
            <p>Documento: {patient.documentNumber}</p>
            <p>EPS: {patient.epsCode}</p>
            <p>Última visita: {patient.lastVisit}</p>
          </div>
        ))}
      </div>
    </div>
  );
};

export default ClinicalSearch;
```
```scss
// ClinicalSearch.module.scss — SASS con variables y anidamiento
@import '../../styles/variables';
@import '../../styles/mixins';

.searchContainer {
  width: 100%;
  max-width: 900px;
  margin: 0 auto;
  padding: $spacing-lg;

  .searchBar {
    display: flex;
    gap: $spacing-sm;
    margin-bottom: $spacing-md;

    .searchInput {
      flex: 1;
      padding: $spacing-sm $spacing-md;
      border: 2px solid $color-border;
      border-radius: $border-radius-md;
      font-size: $font-size-base;
      transition: border-color 0.2s ease;

      &:focus {
        outline: none;
        border-color: $color-primary;
        box-shadow: 0 0 0 3px rgba($color-primary, 0.15);
      }
    }

    .searchButton {
      @include button-primary;
      min-width: 120px;

      &:disabled {
        opacity: 0.6;
        cursor: not-allowed;
      }
    }
  }

  .suggestionList {
    @include dropdown-list;
    margin-bottom: $spacing-md;

    .suggestionItem {
      padding: $spacing-xs $spacing-md;
      cursor: pointer;
      transition: background-color 0.15s ease;

      &:hover {
        background-color: $color-hover;
      }
    }
  }

  .resultCard {
    @include card;
    margin-bottom: $spacing-sm;

    h3 {
      color: $color-primary;
      margin-bottom: $spacing-xs;
    }
  }
}
```

---

### 🔐 Spring Security — JWT + Control de Acceso por Roles
```java
// Configuración de Seguridad — Java 11 + Spring Security 5.7.x
@Configuration
@EnableWebSecurity
@EnableGlobalMethodSecurity(prePostEnabled = true)
public class SecurityConfig extends WebSecurityConfigurerAdapter {

    private final JwtAuthenticationFilter jwtAuthFilter;
    private final CustomUserDetailsService userDetailsService;

    public SecurityConfig(JwtAuthenticationFilter jwtAuthFilter,
                          CustomUserDetailsService userDetailsService) {
        this.jwtAuthFilter = jwtAuthFilter;
        this.userDetailsService = userDetailsService;
    }

    @Override
    protected void configure(HttpSecurity http) throws Exception {
        http
            .csrf().disable()
            .sessionManagement()
                .sessionCreationPolicy(SessionCreationPolicy.STATELESS)
            .and()
            .authorizeRequests()
                .antMatchers("/api/auth/**").permitAll()
                .antMatchers("/actuator/health").permitAll()
                .antMatchers("/api/admin/**").hasRole("ADMIN")
                .antMatchers(HttpMethod.GET, "/api/patients/**")
                    .hasAnyRole("ADMIN", "PHYSICIAN", "NURSE")
                .antMatchers(HttpMethod.POST, "/api/patients/**")
                    .hasAnyRole("ADMIN", "RECEPTIONIST")
                .antMatchers("/api/records/**")
                    .hasAnyRole("ADMIN", "PHYSICIAN")
                .antMatchers("/api/billing/**")
                    .hasAnyRole("ADMIN", "BILLING")
                .antMatchers("/api/search/**")
                    .hasAnyRole("ADMIN", "PHYSICIAN", "NURSE", "BILLING")
                .anyRequest().authenticated()
            .and()
            .addFilterBefore(jwtAuthFilter, UsernamePasswordAuthenticationFilter.class);
    }

    @Bean
    @Override
    public AuthenticationManager authenticationManagerBean() throws Exception {
        return super.authenticationManagerBean();
    }

    @Bean
    public PasswordEncoder passwordEncoder() {
        return new BCryptPasswordEncoder(12);
    }
}
```
```java
// Proveedor de Token JWT — Java 11
@Component
@Slf4j
public class JwtTokenProvider {

    @Value("${jwt.secret}")
    private String jwtSecret;

    @Value("${jwt.expiration}")
    private long jwtExpiration;

    @Value("${jwt.refresh-expiration}")
    private long refreshExpiration;

    public String generateToken(UserDetails userDetails) {
        Map<String, Object> claims = new HashMap<>();
        Collection<? extends GrantedAuthority> roles = userDetails.getAuthorities();
        claims.put("roles", roles.stream()
            .map(GrantedAuthority::getAuthority)
            .collect(Collectors.toList()));

        return Jwts.builder()
            .setClaims(claims)
            .setSubject(userDetails.getUsername())
            .setIssuedAt(new Date())
            .setExpiration(new Date(System.currentTimeMillis() + jwtExpiration))
            .signWith(SignatureAlgorithm.HS512, jwtSecret)
            .compact();
    }

    public String generateRefreshToken(UserDetails userDetails) {
        return Jwts.builder()
            .setSubject(userDetails.getUsername())
            .setIssuedAt(new Date())
            .setExpiration(new Date(System.currentTimeMillis() + refreshExpiration))
            .signWith(SignatureAlgorithm.HS512, jwtSecret)
            .compact();
    }

    public boolean validateToken(String token) {
        try {
            Jwts.parser().setSigningKey(jwtSecret).parseClaimsJws(token);
            return true;
        } catch (ExpiredJwtException e) {
            log.warn("Token JWT expirado: {}", e.getMessage());
        } catch (UnsupportedJwtException | MalformedJwtException | 
                 SignatureException | IllegalArgumentException e) {
            log.error("Token JWT inválido: {}", e.getMessage());
        }
        return false;
    }

    public String getUsernameFromToken(String token) {
        return Jwts.parser()
            .setSigningKey(jwtSecret)
            .parseClaimsJws(token)
            .getBody()
            .getSubject();
    }
}
```

---

### 🧾 Facturación Electrónica DIAN — Servicios de Salud
```java
// Servicio de Facturación — Integración DIAN para servicios médicos
@Service
@Slf4j
public class MedicalBillingService {

    private final DianWebServiceClient dianClient;
    private final DigitalSignatureService signatureService;
    private final InvoiceRepository invoiceRepository;
    private final KafkaTemplate<String, Object> kafkaTemplate;

    @Transactional
    public DianResponse issueElectronicInvoice(MedicalInvoiceDTO invoiceDTO) {
        MedicalInvoice invoice = buildInvoice(invoiceDTO);

        try {
            // 1. Generar XML UBL 2.1 para servicios de salud
            String xmlContent = generateHealthInvoiceXML(invoice);

            // 2. Calcular CUFE
            String cufe = calculateCUFE(invoice);
            invoice.setCufe(cufe);

            // 3. Firmar con certificado digital
            String signedXml = signatureService.signXML(xmlContent);

            // 4. Enviar a DIAN
            DianRequest dianRequest = DianRequest.builder()
                .nit(invoice.getClinic().getNit())
                .invoiceNumber(invoice.getNumber())
                .xmlContent(Base64.getEncoder()
                    .encodeToString(signedXml.getBytes(StandardCharsets.UTF_8)))
                .build();

            ResponseEntity<DianResponse> response =
                dianClient.submitInvoice(dianRequest);

            DianResponse dianResponse = response.getBody();

            if (dianResponse != null && dianResponse.isApproved()) {
                invoice.setDianStatus(DianStatus.APPROVED);
                invoice.setCude(dianResponse.getCude());
                invoice.setQrCode(dianResponse.getQrCode());

                kafkaTemplate.send("billing.issued", 
                    new BillingIssuedEvent(invoice.getId(), invoice.getPatientId()));

                log.info("Factura médica {} aprobada por DIAN", invoice.getNumber());
            } else {
                invoice.setDianStatus(DianStatus.REJECTED);
                if (dianResponse != null) {
                    invoice.setRejectionReason(dianResponse.getErrors());
                }
                log.error("Factura médica {} rechazada por DIAN", invoice.getNumber());
            }

            invoiceRepository.save(invoice);
            return dianResponse;

        } catch (Exception e) {
            log.error("Error de envío DIAN para factura {}", invoice.getNumber(), e);
            throw new BillingServiceException("Integración DIAN fallida", e);
        }
    }

    private String calculateCUFE(MedicalInvoice invoice) {
        // CUFE = SHA-384(Num + Fecha + Hora + SubTotal + Impuesto + Total
        //               + NitEmisor + DocAdquiriente + ClaveTécnica + Ambiente)
        String raw = String.format("%s%s%s%s%s%s%s%s%s%s",
            invoice.getNumber(),
            invoice.getIssueDate().format(DateTimeFormatter.ISO_DATE),
            invoice.getIssueTime().format(DateTimeFormatter.ISO_TIME),
            invoice.getSubtotal().toPlainString(),
            invoice.getTaxAmount().toPlainString(),
            invoice.getTotal().toPlainString(),
            invoice.getClinic().getNit(),
            invoice.getPatient().getDocumentNumber(),
            invoice.getTechnicalKey(),
            invoice.getEnvironment()
        );
        return DigestUtils.sha384Hex(raw);
    }
}
```

---

### 🧪 TDD/BDD — Estrategia de Pruebas
```java
// Backend — JUnit 5 + Mockito (TDD)
@ExtendWith(MockitoExtension.class)
class ClinicalSearchServiceTest {

    @Mock
    private ElasticsearchOperations elasticsearchOperations;

    @Mock
    private RestHighLevelClient restHighLevelClient;

    @InjectMocks
    private ClinicalSearchService clinicalSearchService;

    @Test
    @DisplayName("Debe retornar resultados de pacientes cuando se proporciona un término válido")
    void shouldReturnPatientHitsWhenValidSearchTerm() {
        // Dado
        ClinicalSearchRequest request = ClinicalSearchRequest.builder()
            .term("Maria Garcia")
            .page(0)
            .size(10)
            .build();

        PatientDocument document = PatientDocument.builder()
            .id("1")
            .fullName("Maria Garcia Lopez")
            .documentNumber("12345678")
            .build();

        SearchHits<PatientDocument> mockHits =
            new SearchHitsImpl<>(1L, TotalHitsRelation.EQUAL_TO, 1.0f,
                null, List.of(new SearchHit<>("patients", "1", null,
                    1.0f, null, null, null, null, null, null, document)),
                null, null);

        when(elasticsearchOperations.search(
            any(NativeSearchQuery.class), eq(PatientDocument.class)))
            .thenReturn(mockHits);

        // Cuando
        SearchHits<PatientDocument> result =
            clinicalSearchService.searchPatients(request);

        // Entonces
        assertThat(result.getTotalHits()).isEqualTo(1L);
        assertThat(result.getSearchHit(0).getContent().getFullName())
            .isEqualTo("Maria Garcia Lopez");

        verify(elasticsearchOperations, times(1))
            .search(any(NativeSearchQuery.class), eq(PatientDocument.class));
    }

    @Test
    @DisplayName("Debe indexar documento de paciente cuando se recibe evento Kafka")
    void shouldIndexPatientWhenKafkaEventReceived() {
        // Dado
        PatientIndexEvent event = PatientIndexEvent.builder()
            .patientId(UUID.randomUUID())
            .fullName("Carlos Perez")
            .documentNumber("98765432")
            .epsCode("SURA-001")
            .lastVisit(LocalDate.now())
            .build();

        // Cuando
        clinicalSearchService.indexPatientEvent(event);

        // Entonces
        ArgumentCaptor<PatientDocument> captor =
            ArgumentCaptor.forClass(PatientDocument.class);
        verify(elasticsearchOperations, times(1)).save(captor.capture());

        PatientDocument saved = captor.getValue();
        assertThat(saved.getFullName()).isEqualTo("Carlos Perez");
        assertThat(saved.getEpsCode()).isEqualTo("SURA-001");
        assertThat(saved.isActive()).isTrue();
    }
}
```
```typescript
// Frontend — React Testing Library (TDD)
import { render, screen, fireEvent, waitFor } from '@testing-library/react';
import { Provider } from 'react-redux';
import { configureStore } from '@reduxjs/toolkit';
import ClinicalSearch from './ClinicalSearch';
import searchReducer from '../../store/slices/searchSlice';
import * as searchService from '../../services/searchService';

jest.mock('../../services/searchService');

const renderWithStore = (preloadedState = {}) => {
  const store = configureStore({
    reducer: { search: searchReducer },
    preloadedState,
  });
  return render(
    <Provider store={store}>
      <ClinicalSearch />
    </Provider>
  );
};

describe('Componente ClinicalSearch', () => {
  beforeEach(() => {
    jest.clearAllMocks();
  });

  it('debe renderizar el campo de búsqueda y el botón', () => {
    renderWithStore();
    expect(screen.getByTestId('clinical-search-input')).toBeInTheDocument();
    expect(screen.getByTestId('search-button')).toBeInTheDocument();
  });

  it('debe despachar el thunk searchPatients al hacer clic en el botón', async () => {
    (searchService.searchPatients as jest.Mock).mockResolvedValue({
      data: { hits: [], totalHits: 0 },
    });

    renderWithStore();

    const input = screen.getByTestId('clinical-search-input');
    const button = screen.getByTestId('search-button');

    fireEvent.change(input, { target: { value: 'Maria Garcia' } });
    fireEvent.click(button);

    await waitFor(() => {
      expect(searchService.searchPatients).toHaveBeenCalledWith(
        expect.objectContaining({ term: 'Maria Garcia' })
      );
    });
  });

  it('debe mostrar mensaje de error cuando la búsqueda falla', async () => {
    (searchService.searchPatients as jest.Mock).mockRejectedValue({
      response: { data: { message: 'Servicio de búsqueda no disponible' } },
    });

    renderWithStore();

    fireEvent.change(screen.getByTestId('clinical-search-input'), {
      target: { value: 'consulta de prueba' },
    });
    fireEvent.click(screen.getByTestId('search-button'));

    await waitFor(() => {
      expect(screen.getByTestId('search-error')).toHaveTextContent(
        'Servicio de búsqueda no disponible'
      );
    });
  });
});
```
```javascript
// Cypress BDD — Flujo E2E de Búsqueda Clínica
describe('Búsqueda Clínica - BDD', () => {
  beforeEach(() => {
    cy.login('physician@clinicacore.com', 'testpass');
    cy.visit('/dashboard/search');
  });

  it('Dado un médico, Cuando busca un paciente por nombre, Entonces deben aparecer resultados', () => {
    // Dado
    cy.intercept('GET', '/api/search/patients*', {
      fixture: 'search-results.json',
    }).as('searchRequest');

    // Cuando
    cy.get('[data-testid="clinical-search-input"]').type('Maria Garcia');
    cy.get('[data-testid="search-button"]').click();

    // Entonces
    cy.wait('@searchRequest');
    cy.get('[data-testid="patient-result-card"]').should('have.length.greaterThan', 0);
    cy.get('[data-testid="patient-result-card"]').first()
      .should('contain.text', 'Maria Garcia');
  });

  it('Dado un médico, Cuando escribe un prefijo de diagnóstico, Entonces aparecen sugerencias CIE-10', () => {
    cy.intercept('GET', '/api/search/autocomplete*', {
      body: ['Diabetes mellitus tipo 2', 'Diabetes insípida'],
    }).as('autocompleteRequest');

    cy.get('[data-testid="clinical-search-input"]').type('Diab');

    cy.wait('@autocompleteRequest');
    cy.get('[data-testid="suggestions-list"]').should('be.visible');
    cy.get('[data-testid="suggestions-list"] li').should('have.length', 2);
  });
});
```

---

## 📊 Modelo de Datos

### Esquema Principal — PostgreSQL
```sql
-- Pacientes
CREATE TABLE patients (
    id                  BIGSERIAL PRIMARY KEY,
    document_type       VARCHAR(20) NOT NULL,         -- CC, CE, PA, RC, TI
    document_number     VARCHAR(50) UNIQUE NOT NULL,
    first_name          VARCHAR(100) NOT NULL,
    last_name           VARCHAR(100) NOT NULL,
    birth_date          DATE NOT NULL,
    gender              VARCHAR(10) NOT NULL,
    blood_type          VARCHAR(5),
    phone               VARCHAR(20),
    email               VARCHAR(255),
    address             TEXT,
    city                VARCHAR(100),
    department          VARCHAR(100),
    eps_code            VARCHAR(50),
    eps_name            VARCHAR(255),
    beneficiary_type    VARCHAR(30),                  -- COTIZANTE, BENEFICIARIO, SUBSIDIADO
    consent_signed      BOOLEAN DEFAULT FALSE,
    consent_date        TIMESTAMP,
    active              BOOLEAN DEFAULT TRUE,
    created_at          TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    updated_at          TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

-- Médicos
CREATE TABLE physicians (
    id                  BIGSERIAL PRIMARY KEY,
    document_number     VARCHAR(50) UNIQUE NOT NULL,
    full_name           VARCHAR(255) NOT NULL,
    medical_card        VARCHAR(50) UNIQUE NOT NULL,  -- Tarjeta Profesional
    specialty           VARCHAR(100) NOT NULL,
    sub_specialty       VARCHAR(100),
    email               VARCHAR(255) UNIQUE NOT NULL,
    phone               VARCHAR(20),
    active              BOOLEAN DEFAULT TRUE,
    created_at          TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

-- Citas
CREATE TABLE appointments (
    id                  BIGSERIAL PRIMARY KEY,
    patient_id          BIGINT REFERENCES patients(id) NOT NULL,
    physician_id        BIGINT REFERENCES physicians(id) NOT NULL,
    appointment_date    DATE NOT NULL,
    appointment_time    TIME NOT NULL,
    duration_minutes    INTEGER DEFAULT 30,
    specialty           VARCHAR(100) NOT NULL,
    appointment_type    VARCHAR(30) NOT NULL,          -- FIRST_VISIT, CONTROL, EMERGENCY
    status              VARCHAR(20) DEFAULT 'SCHEDULED', -- SCHEDULED, CONFIRMED, COMPLETED, CANCELLED, NO_SHOW
    cancellation_reason TEXT,
    notes               TEXT,
    created_by          BIGINT REFERENCES users(id),
    created_at          TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    updated_at          TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

-- Historia Clínica Electrónica (Res. 1995/1999)
CREATE TABLE medical_records (
    id                  BIGSERIAL PRIMARY KEY,
    patient_id          BIGINT REFERENCES patients(id) NOT NULL,
    appointment_id      BIGINT REFERENCES appointments(id),
    physician_id        BIGINT REFERENCES physicians(id) NOT NULL,
    record_date         TIMESTAMP NOT NULL,
    version             INTEGER NOT NULL DEFAULT 1,    -- Versionado inmutable
    is_current          BOOLEAN DEFAULT TRUE,
    reason_for_visit    TEXT NOT NULL,
    current_illness     TEXT,
    vital_signs         JSONB,                         -- FC, TA, Temp, Peso, Talla
    physical_exam       TEXT,
    clinical_impression TEXT,
    treatment_plan      TEXT,
    observations        TEXT,
    created_at          TIMESTAMP DEFAULT CURRENT_TIMESTAMP
    -- No se permite UPDATE — cada cambio genera una nueva versión
);

-- Diagnósticos (CIE-10)
CREATE TABLE diagnoses (
    id                  BIGSERIAL PRIMARY KEY,
    record_id           BIGINT REFERENCES medical_records(id) NOT NULL,
    icd10_code          VARCHAR(10) NOT NULL,
    icd10_description   VARCHAR(500) NOT NULL,
    diagnosis_type      VARCHAR(20) NOT NULL,          -- PRINCIPAL, RELACIONADO, COMPLICACION
    diagnosis_status    VARCHAR(20) NOT NULL,          -- CONFIRMADO, SOSPECHOSO, DESCARTADO
    created_at          TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

-- Prescripciones
CREATE TABLE prescriptions (
    id                  BIGSERIAL PRIMARY KEY,
    record_id           BIGINT REFERENCES medical_records(id) NOT NULL,
    patient_id          BIGINT REFERENCES patients(id) NOT NULL,
    physician_id        BIGINT REFERENCES physicians(id) NOT NULL,
    issue_date          DATE NOT NULL,
    valid_until         DATE,
    status              VARCHAR(20) DEFAULT 'ACTIVE',  -- ACTIVE, DISPENSED, EXPIRED, CANCELLED
    created_at          TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

-- Ítems de Prescripción
CREATE TABLE prescription_items (
    id                  BIGSERIAL PRIMARY KEY,
    prescription_id     BIGINT REFERENCES prescriptions(id) ON DELETE CASCADE,
    medication_name     VARCHAR(255) NOT NULL,
    active_ingredient   VARCHAR(255),
    dosage              VARCHAR(100) NOT NULL,
    frequency           VARCHAR(100) NOT NULL,
    duration            VARCHAR(100),
    quantity            INTEGER,
    administration_route VARCHAR(50),
    instructions        TEXT
);

-- Facturas Médicas
CREATE TABLE medical_invoices (
    id                  BIGSERIAL PRIMARY KEY,
    invoice_number      VARCHAR(50) UNIQUE NOT NULL,
    patient_id          BIGINT REFERENCES patients(id) NOT NULL,
    appointment_id      BIGINT REFERENCES appointments(id),
    issue_date          DATE NOT NULL,
    due_date            DATE,
    subtotal            DECIMAL(12,2) NOT NULL,
    tax_amount          DECIMAL(12,2) NOT NULL DEFAULT 0,
    total               DECIMAL(12,2) NOT NULL,
    payment_status      VARCHAR(20) DEFAULT 'PENDING', -- PENDING, PARTIAL, PAID
    payment_method      VARCHAR(30),

    -- Campos DIAN
    dian_status         VARCHAR(20),                   -- PENDING, APPROVED, REJECTED
    cufe                VARCHAR(255),
    cude                VARCHAR(255),
    qr_code             TEXT,
    dian_xml            TEXT,
    dian_response       TEXT,
    dian_sent_at        TIMESTAMP,
    technical_key       VARCHAR(255),
    environment         VARCHAR(10) DEFAULT 'PRODUCTION',

    created_by          BIGINT REFERENCES users(id),
    created_at          TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

-- Ítems de Factura
CREATE TABLE invoice_items (
    id                  BIGSERIAL PRIMARY KEY,
    invoice_id          BIGINT REFERENCES medical_invoices(id) ON DELETE CASCADE,
    service_code        VARCHAR(50),                   -- Código CUPS (salud colombiana)
    service_description VARCHAR(500) NOT NULL,
    quantity            INTEGER NOT NULL DEFAULT 1,
    unit_price          DECIMAL(10,2) NOT NULL,
    tax_rate            DECIMAL(5,2) DEFAULT 0,
    tax_amount          DECIMAL(10,2) DEFAULT 0,
    subtotal            DECIMAL(10,2) NOT NULL
);

-- Control de Sincronización ElasticSearch
CREATE TABLE es_sync_log (
    id                  BIGSERIAL PRIMARY KEY,
    entity_type         VARCHAR(50) NOT NULL,          -- PATIENT, RECORD, DIAGNOSIS
    entity_id           BIGINT NOT NULL,
    sync_status         VARCHAR(20) DEFAULT 'PENDING', -- PENDING, SYNCED, FAILED
    retry_count         INTEGER DEFAULT 0,
    last_attempt        TIMESTAMP,
    error_message       TEXT,
    created_at          TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

-- Log de Auditoría
CREATE TABLE audit_log (
    id                  BIGSERIAL PRIMARY KEY,
    user_id             BIGINT REFERENCES users(id),
    action              VARCHAR(50) NOT NULL,           -- CREATE, READ, UPDATE, DELETE
    entity_type         VARCHAR(50) NOT NULL,
    entity_id           BIGINT,
    old_value           JSONB,
    new_value           JSONB,
    ip_address          VARCHAR(45),
    user_agent          TEXT,
    created_at          TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

-- Índices para rendimiento
CREATE INDEX idx_patients_document ON patients(document_number);
CREATE INDEX idx_patients_eps ON patients(eps_code);
CREATE INDEX idx_appointments_date ON appointments(appointment_date, physician_id);
CREATE INDEX idx_appointments_patient ON appointments(patient_id, status);
CREATE INDEX idx_records_patient ON medical_records(patient_id, is_current);
CREATE INDEX idx_diagnoses_icd10 ON diagnoses(icd10_code);
CREATE INDEX idx_invoices_dian_status ON medical_invoices(dian_status);
CREATE INDEX idx_audit_entity ON audit_log(entity_type, entity_id);
```

### Esquema NoSQL — MongoDB (Logs Clínicos y Datos No Estructurados)
```javascript
// Log de Eventos Clínicos — Colección MongoDB
{
  _id: ObjectId,
  eventType: String,        // "PATIENT_VIEWED", "RECORD_ACCESSED", "SEARCH_EXECUTED"
  patientId: Number,
  userId: Number,
  userRole: String,
  sessionId: String,
  ipAddress: String,
  metadata: {
    searchTerm: String,
    resultsCount: Number,
    responseTimeMs: Number,
    elasticQuery: Object
  },
  timestamp: ISODate,
  environment: String
}

// Log de Consultas Raw de ElasticSearch — Colección MongoDB
{
  _id: ObjectId,
  serviceId: String,
  queryType: String,        // "FULL_TEXT", "AUTOCOMPLETE", "AGGREGATION"
  indexName: String,
  rawQuery: Object,
  hitsCount: Number,
  tookMs: Number,
  timestamp: ISODate
}
```

---

## 🎓 Aprendizajes Clave

### Arquitectura y Backend

✅ **Microservicios con Java 11** — Diseño e implementación de servicios distribuidos
usando Spring Boot 2.7.x y Spring MVC 5.3.x, manteniendo compatibilidad con
Java 11 LTS, demostrando dominio en diferentes versiones del ecosistema Java

✅ **ElasticSearch 7.x** — Búsqueda clínica de texto completo con analizadores
personalizados, potenciación multi-campo, coincidencia difusa, autocompletado CIE-10,
y gestión del ciclo de vida de índices (ILM) para políticas de retención de datos de auditoría

✅ **Indexación Orientada a Eventos** — Sincronización de índices impulsada por Kafka
que garantiza que ElasticSearch siempre refleje el estado más reciente en todos los
microservicios sin acoplar los límites de servicio

✅ **Spring Cloud** — API Gateway con validación JWT, descubrimiento de servicios
con Eureka, Config Server para gestión centralizada de entornos, y Resilience4j para
circuit breaking bajo escenarios de alta carga de pacientes

✅ **Historias Clínicas Inmutables** — Implementación de estrategia de versionado de HCE
en cumplimiento con la Resolución 1995/1999: ningún registro se sobrescribe jamás,
cada cambio clínico genera una nueva versión inmutable con trazabilidad completa

✅ **TDD con JUnit 5 + Mockito** — Desarrollo guiado por pruebas en todas las capas
de servicio, logrando alta cobertura en rutas críticas de negocio incluyendo integración
DIAN, consultas ElasticSearch y procesamiento de eventos Kafka

### Frontend y UX

✅ **React 18 + TypeScript** — SPA tipada construida con componentes funcionales,
hooks personalizados y configuración TypeScript estricta para detección temprana de errores

✅ **Redux Toolkit + Thunks** — Gestión de estado centralizada implementada con slices
de Redux Toolkit y middleware Redux Thunk para todas las operaciones asíncronas,
reemplazando los patrones legacy connect() con hooks useSelector/useDispatch

✅ **Configuración Manual de Webpack 5** — Bundle Webpack de nivel producción configurado
manualmente: división de código por ruta, tree shaking, carga diferida, compilación
SASS, optimización de assets y builds específicos por entorno sin abstracciones
de Create React App

✅ **Arquitectura SASS** — Base de código SASS estructurada siguiendo el patrón 7-1:
variables globales, mixins, tipografía, layout y módulos con alcance de componente
para un sistema de diseño consistente en más de 20 componentes de UI clínica

✅ **React Testing Library + Cypress** — TDD aplicado en componentes usando el enfoque
centrado en el usuario de Testing Library, y escenarios E2E con BDD en Cypress
cubriendo flujos clínicos críticos: búsqueda de pacientes, agendamiento de citas
y flujos de acceso a historias clínicas

### Integraciones y Cumplimiento Normativo

✅ **Facturación Electrónica DIAN** — Integración de facturación electrónica colombiana
para servicios de salud con generación de XML UBL 2.1, cálculo de CUFE,
firma digital PKCS#12 y validación DIAN en tiempo real para códigos de
servicios médicos (CUPS)

✅ **Generación de RIPS** — Reportes RIPS automatizados (Registro Individual de
Prestación de Servicios) exigidos por MinSalud para el cobro a EPS, agregando
datos de cuatro microservicios con validación estricta de formato

✅ **Normatividad Colombiana de Salud** — Implementación profunda de la Resolución
1995/1999 (historia clínica electrónica), codificación diagnóstica CIE-10,
codificación de procedimientos CUPS y validación de beneficiarios ADRES

### DevOps y Operaciones

✅ **CI/CD con Jenkins** — Pipelines declarativos configurados que cubren:
compilación, pruebas unitarias, pruebas de integración, construcción de imagen
Docker, publicación en registro y despliegue en Kubernetes con rollback ante fallos

✅ **ELK Stack** — Stack de observabilidad completo: pipelines Logstash para
ingestión estructurada de eventos clínicos, ElasticSearch como motor de búsqueda
y almacén de logs, dashboards Kibana para monitoreo operacional y analítica de búsqueda

✅ **Kubernetes en Producción** — Despliegues multi-servicio en Kubernetes gestionados
con ConfigMaps, Secrets, autoescalado horizontal de pods, sondas de liveness y
readiness, y actualizaciones rolling sin tiempo de inactividad

---

## 🔄 Roadmap y Mejoras Futuras

### Fase 2 — Q2 2025
- Aplicación móvil para médicos (React Native) con acceso offline a HCE
- Sugerencia de diagnóstico asistida por IA basada en síntomas (NLP + ElasticSearch MLkit)
- Módulo de telemedicina con consulta por video WebRTC
- Recordatorios de citas por WhatsApp vía Twilio

### Fase 3 — Q3 2025
- Soporte multi-tenant para redes de clínicas y grupos hospitalarios
- Integración con el sistema nacional PISIS (reporte epidemiológico MinSalud)
- Sugerencias automatizadas de codificación CIE-10 desde notas clínicas usando ML
- Prescripción electrónica con integración a cadenas de farmacias

### Fase 4 — Q4 2025
- Dashboard de analítica predictiva para tasas de inasistencia de pacientes
- Integración IoT con dispositivos de signos vitales (glucómetros, oxímetros)
- Portabilidad de datos del paciente entre clínicas con consentimiento del paciente
- API FHIR R4 para interoperabilidad con sistemas externos de salud

---

## 📈 Impacto y Resultados

Este sistema ha sido implementado y validado en **consultorios médicos privados y
clínicas colombianas** a través de SENA, generando mejoras operacionales medibles:

✅ **85% de reducción** en el tiempo de búsqueda de pacientes usando ElasticSearch
vs búsqueda SQL tradicional
✅ **Tasa de aprobación DIAN superior al 95%** en el primer envío para facturas
de servicios médicos
✅ **100% de cumplimiento normativo** con los requisitos de historia clínica
electrónica de la Resolución 1995/1999
✅ **Cero errores manuales en RIPS** — la generación automatizada eliminó los
errores de transcripción
✅ **60% de reducción** en el tiempo de agendamiento de citas mediante gestión
de calendario en tiempo real
✅ **Trazabilidad completa** en cada acceso a historia clínica, cumpliendo los
requisitos de HABEAS DATA

---

## 📄 Licencia y Propiedad Intelectual

> ⚠️ **SENA — Aviso de Propiedad Intelectual**
>
> Este proyecto fue desarrollado como parte de actividades académicas e investigativas
> dentro del **SENA (Servicio Nacional de Aprendizaje)** bajo el programa **SENNOVA**,
> orientado a apoyar la transformación digital de microempresas colombianas del
> sector salud.
>
> El **código fuente, diseño arquitectónico y documentación técnica son propiedad
> institucional del SENA** y no se encuentran disponibles públicamente en este
> repositorio. El contenido presentado aquí — incluyendo especificaciones técnicas,
> diagramas de arquitectura, fragmentos de código y modelos de datos — ha sido
> **recreado con fines de demostración de portafolio únicamente**, sin exponer
> información institucional confidencial ni el código fuente original de producción.
>
> Las capturas de pantalla e imágenes de la interfaz han sido intencionalmente
> excluidas para proteger la privacidad de los datos de los pacientes y la
> confidencialidad institucional.

**Disponible para:**

✅ Consultoría personalizada e implementación para clínicas privadas
✅ Capacitación en facturación electrónica DIAN para prestadores de salud
✅ Diseño y optimización de arquitecturas de búsqueda con ElasticSearch
✅ Asesoría en cumplimiento normativo colombiano (Res. 1995/1999, RIPS)
✅ Desarrollo de módulos adicionales y soporte técnico

---

## 🏆 Reconocimientos

- 🥇 **Mejor Proyecto de Innovación en Salud** — SENA Regional Risaralda 2024
- 🏅 **Certificación DIAN** — Sistema validado para facturación electrónica de
servicios de salud
- 🏥 **Alineación MinSalud** — Arquitectura validada frente a la Resolución 1995/1999
- ⭐ **Implementado en más de 10 consultorios médicos privados colombianos**


| **Swagger/OpenAPI** | Documentación de API y pruebas interactivas                      |
| **JUnit 5**         | Pruebas unitarias e integración del backend (TDD)                |
| **Mockito**         | Simulación de dependencias para pruebas unitarias aisladas       |
