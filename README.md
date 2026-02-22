# ClinicaCore 🏥

Comprehensive healthcare management platform built on **microservices architecture**, 
designed to digitize and optimize clinical and medical office operations. 
Integrates advanced patient record management, intelligent appointment scheduling, 
DIAN-compliant electronic billing for healthcare services, and 
**high-performance search capabilities powered by ElasticSearch** across all clinical data.

Developed for Colombian private clinics and medical offices requiring compliance with 
**Resolución 1995/1999** (electronic health records), **RIPS reporting** for EPS entities, 
and DIAN electronic invoicing regulations.

---

## 🛠️ Complete Technology Stack

### Backend — Microservices

| Technology          | Version   | Purpose                                      |
|---------------------|-----------|----------------------------------------------|
| **Java**            | 11        | Core backend language                        |
| **Spring Boot**     | 2.7.x     | Microservices framework (Java 11 compatible) |
| **Spring MVC**      | 5.3.x     | REST controllers and request handling        |
| **Spring Cloud**    | 2021.x    | Distributed systems support                  |
| **Spring Cloud Gateway** | —    | API Gateway with rate limiting               |
| **Eureka Server**   | —         | Service Discovery                            |
| **Spring Cloud Config** | —     | Centralized configuration management         |
| **Spring Data JPA** | —         | ORM and database persistence                 |
| **Spring Security** | 5.7.x     | Authentication and authorization             |
| **Resilience4j**    | 1.7.x     | Circuit Breaker and Rate Limiting            |
| **Apache Kafka**    | 3.x       | Asynchronous event-driven messaging          |
| **ElasticSearch**   | 7.17.x    | Full-text search and clinical data indexing  |
| **Logstash**        | 7.17.x    | Data pipeline into ElasticSearch             |
| **Kibana**          | 7.17.x    | Search analytics and log visualization       |
| **PostgreSQL**      | 15+       | Primary relational database                  |
| **MongoDB**         | 6.x       | Clinical logs and unstructured documents     |
| **Redis**           | 7.x       | Distributed cache and session management     |

### Frontend — Single Page Application

| Technology              | Version | Purpose                                      |
|-------------------------|---------|----------------------------------------------|
| **React**               | 18.x    | Frontend UI framework                        |
| **TypeScript**          | 5.x     | Static typing and OOP                        |
| **React-Redux**         | 8.x     | Global state management                      |
| **Redux Toolkit**       | 1.9.x   | Simplified Redux with slices                 |
| **Redux Thunk**         | 2.4.x   | Async middleware for API calls               |
| **React Router v6**     | 6.x     | Client-side routing                          |
| **Webpack**             | 5.x     | Manual module bundling and optimization      |
| **SASS/SCSS**           | 1.x     | Advanced CSS preprocessing                   |
| **Axios**               | 1.x     | HTTP client with interceptors                |
| **React Testing Library** | 14.x  | Component unit testing (TDD)                 |
| **Cypress**             | 13.x    | End-to-end testing (BDD)                     |
| **Chart.js**            | 4.x     | Clinical dashboards and analytics charts     |
| **Socket.io Client**    | 4.x     | Real-time WebSocket updates                  |

### External Integrations

| System                    | Purpose                                           |
|---------------------------|---------------------------------------------------|
| **DIAN Web Services**     | Official electronic invoicing for health services |
| **DIAN RADIAN**           | Electronic invoice events management              |
| **RIPS (MinSalud)**       | Mandatory health records reporting for EPS        |
| **ADRES API**             | Beneficiary and coverage validation               |
| **PayU / Mercado Pago**   | Payment gateway integration                       |

### DevOps & Infrastructure

| Technology          | Purpose                                           |
|---------------------|---------------------------------------------------|
| **Docker**          | Service containerization                          |
| **Docker Compose**  | Local multi-service orchestration                 |
| **Kubernetes**      | Production-grade container orchestration          |
| **Jenkins**         | CI/CD pipeline automation                         |
| **Prometheus**      | Metrics collection and alerting                   |
| **Grafana**         | Infrastructure and application monitoring         |
| **ELK Stack**       | Centralized logging (ElasticSearch + Logstash + Kibana) |
| **Swagger/OpenAPI** | API documentation and interactive testing         |
| **JUnit 5**         | Backend unit and integration testing (TDD)        |
| **Mockito**         | Dependency mocking for isolated unit tests        |

---

## 🏗️ Microservices Architecture
```
                    ┌─────────────────────────────────────────┐
                    │         React 18 Frontend SPA           │
                    │  (Redux + Thunks · SASS · Webpack 5)   │
                    └─────────────────────────────────────────┘
                                        ↓
                    ┌─────────────────────────────────────────┐
                    │     API Gateway (Spring Cloud Gateway)  │
                    │  - JWT Validation                       │
                    │  - Rate Limiting (Resilience4j)         │
                    │  - Load Balancing                       │
                    │  - Route Filtering                      │
                    └─────────────────────────────────────────┘
                                        ↓
       ┌──────────────┬─────────────────┬──────────────┬─────────────┬──────────────┐
       ↓              ↓                 ↓              ↓             ↓              ↓
┌───────────┐  ┌────────────┐  ┌──────────────┐  ┌──────────┐ ┌──────────┐ ┌───────────┐
│  Patient  │  │Appointment │  │    Medical   │  │ Billing  │ │  Search  │ │   RIPS    │
│  Service  │  │  Service   │  │  Record Svc  │  │  Service │ │  Service │ │  Service  │
│           │  │            │  │              │  │          │ │          │ │           │
│ - CRUD    │  │ - Schedule │  │ - EHR Mgmt   │  │ - DIAN   │ │ - ES     │ │ - MinSal  │
│ - History │  │ - Calendar │  │ - Diagnoses  │  │ - PDF    │ │   Index  │ │   Report  │
│ - Consent │  │ - Remind.  │  │ - Treatments │  │ - PayU   │ │ - Query  │ │ - EPS     │
└───────────┘  └────────────┘  └──────────────┘  └──────────┘ └──────────┘ └───────────┘
       ↓              ↓                 ↓              ↓             ↓              ↓
┌───────────┐  ┌────────────┐  ┌──────────────┐  ┌──────────┐ ┌──────────┐ ┌───────────┐
│PostgreSQL │  │ PostgreSQL │  │  PostgreSQL  │  │PostgreSQL│ │ElasticSe-│ │  MongoDB  │
│(Patients) │  │(Appoint.)  │  │  (Records)   │  │(Billing) │ │  arch    │ │  (Logs)   │
└───────────┘  └────────────┘  └──────────────┘  └──────────┘ └──────────┘ └───────────┘

                    ┌─────────────────────────────────────────┐
                    │         Apache Kafka Message Bus        │
                    │  Topics:                                │
                    │  - patient.registered                   │
                    │  - appointment.confirmed                │
                    │  - record.updated                       │
                    │  - billing.issued                       │
                    │  - search.index.requested               │
                    │  - rips.report.generated                │
                    └─────────────────────────────────────────┘
                                        ↓
          ┌─────────────────────────────────────────────────────┐
          │              ELK Stack — Observability              │
          │   ElasticSearch · Logstash · Kibana                 │
          │   (Clinical audit logs + search analytics)          │
          └─────────────────────────────────────────────────────┘
                                        ↓
                    ┌─────────────────────────────────────────┐
                    │        DIAN Integration Service         │
                    │  - Digital Signature (PKCS#12)          │
                    │  - UBL 2.1 XML Generation               │
                    │  - CUFE Calculation                     │
                    │  - Real-time DIAN Validation            │
                    └─────────────────────────────────────────┘
                                        ↓
                    ┌─────────────────────────────────────────┐
                    │         DIAN Web Services               │
                    │     (Electronic Invoicing - Health)     │
                    └─────────────────────────────────────────┘
```

---

## ⚙️ Service Responsibilities

### Patient Service
Core service managing all patient lifecycle operations. Handles patient registration 
with full demographic data, consent management under Colombian Law 1581 (data 
protection), medical history versioning, and publishes events to Kafka upon any 
patient state change so downstream services (Search, RIPS) stay synchronized.

### Appointment Service
Manages the full scheduling lifecycle: slot availability by physician and specialty, 
booking with conflict detection, automated reminders via email/SMS, and cancellation 
workflows. Publishes `appointment.confirmed` events consumed by the Medical Record 
Service to pre-create consultation entries.

### Medical Record Service (EHR)
The most sensitive service in the architecture. Stores and versions electronic health 
records (Historia Clínica Electrónica) in compliance with **Resolución 1995/1999**. 
Every record mutation is immutable — updates generate new versions, never overwrite. 
Diagnoses follow ICD-10 coding, and treatments are linked to prescriptions with 
complete audit trail.

### Billing Service
Handles electronic invoicing for medical services with full DIAN integration. Generates 
UBL 2.1-compliant XML, calculates CUFE, manages digital signature, and processes 
payment gateway callbacks from PayU and Mercado Pago. Publishes events that trigger 
automatic accounting entries downstream.

### Search Service (ElasticSearch Core)
The differentiating service of this architecture. Consumes events from all other 
services via Kafka and maintains a denormalized, optimized ElasticSearch index. 
Exposes advanced search endpoints: full-text patient search, diagnostic code lookup, 
medication search with autocomplete, and cross-service clinical analytics queries. 
Implements index lifecycle management (ILM) for audit log retention policies.

### RIPS Service
Generates mandatory RIPS (Registro Individual de Prestación de Servicios) reports 
required by the Colombian Ministry of Health (MinSalud) for EPS billing. Aggregates 
data from Patient, Appointment, and Medical Record services and produces the 
standardized output format for regulatory submission.

---

## ✨ Main Features

### 🔍 ElasticSearch — Advanced Clinical Search

ElasticSearch is the core differentiator of ClinicaCore. Every service publishes 
events to Kafka, which are consumed by the Search Service to maintain a real-time, 
denormalized index optimized for clinical queries. This enables sub-second full-text 
search across patients, diagnoses, treatments, and medical records simultaneously.

**Index Configuration — Patient Clinical Index**
```java
// Search Service — ElasticSearch Index Configuration
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
// Clinical Patient Document — ElasticSearch Index Mapping
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
// Search Service — Clinical Query Engine
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

    // Multi-field full-text patient search
    public SearchHits<PatientDocument> searchPatients(ClinicalSearchRequest request) {
        NativeSearchQueryBuilder queryBuilder = new NativeSearchQueryBuilder();

        // Multi-match across name, document, clinical notes
        MultiMatchQueryBuilder multiMatch = QueryBuilders.multiMatchQuery(
                request.getTerm(),
                "fullName^3",          // boost name relevance
                "documentNumber^5",    // highest priority
                "clinicalNotes",
                "diagnoses.description"
        ).type(MultiMatchQueryBuilder.Type.BEST_FIELDS)
         .fuzziness(Fuzziness.AUTO);   // typo tolerance

        BoolQueryBuilder boolQuery = QueryBuilders.boolQuery()
            .must(multiMatch)
            .filter(QueryBuilders.termQuery("active", true));

        // EPS filter when provided
        if (request.getEpsCode() != null) {
            boolQuery.filter(QueryBuilders.termQuery("epsCode", request.getEpsCode()));
        }

        // Date range filter for last visit
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

        log.info("Executing clinical search: term={}, eps={}", 
            request.getTerm(), request.getEpsCode());

        return elasticsearchOperations.search(searchQuery, PatientDocument.class);
    }

    // Diagnosis autocomplete — ICD-10
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
            log.error("ElasticSearch autocomplete error", e);
            throw new SearchServiceException("Autocomplete failed", e);
        }
    }

    // Index patient from Kafka event
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
        log.info("Patient indexed in ElasticSearch: id={}", event.getPatientId());
    }
}
```

---

### ⚛️ React — Redux Toolkit + Thunks

The frontend follows a strict **Redux architecture** with Redux Toolkit slices and 
Redux Thunk for all asynchronous operations. Webpack 5 is configured manually for 
full control over code splitting, bundle optimization, and SASS compilation — 
no Create React App abstractions.

**Redux Store Setup**
```typescript
// store/index.ts — Redux store with Thunk middleware
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
// store/slices/searchSlice.ts — ElasticSearch Redux integration
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

// Async Thunk — Full-text clinical search
export const searchPatients = createAsyncThunk(
  'search/searchPatients',
  async (params: ClinicalSearchParams, { rejectWithValue }) => {
    try {
      const response = await searchService.searchPatients(params);
      return response.data;
    } catch (error: any) {
      return rejectWithValue(error.response?.data?.message || 'Search failed');
    }
  }
);

// Async Thunk — ICD-10 diagnosis autocomplete
export const fetchDiagnosisSuggestions = createAsyncThunk(
  'search/fetchSuggestions',
  async (prefix: string, { rejectWithValue }) => {
    try {
      const response = await searchService.autocompleteDiagnosis(prefix);
      return response.data;
    } catch (error: any) {
      return rejectWithValue(error.response?.data?.message || 'Autocomplete failed');
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

      // Debounced autocomplete via Thunk
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
          placeholder="Search patients, diagnoses, medications..."
          className={styles.searchInput}
          data-testid="clinical-search-input"
        />
        <button
          onClick={handleSearch}
          disabled={loading}
          className={styles.searchButton}
          data-testid="search-button"
        >
          {loading ? 'Searching...' : 'Search'}
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
          <span>{totalHits} clinical records found</span>
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
            <p>Document: {patient.documentNumber}</p>
            <p>EPS: {patient.epsCode}</p>
            <p>Last visit: {patient.lastVisit}</p>
          </div>
        ))}
      </div>
    </div>
  );
};

export default ClinicalSearch;
```
```scss
// ClinicalSearch.module.scss — SASS with variables and nesting
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

### 🔐 Spring Security — JWT + Role-Based Access
```java
// Security Configuration — Java 11 + Spring Security 5.7.x
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
// JWT Token Provider — Java 11
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
            log.warn("JWT token expired: {}", e.getMessage());
        } catch (UnsupportedJwtException | MalformedJwtException | 
                 SignatureException | IllegalArgumentException e) {
            log.error("Invalid JWT token: {}", e.getMessage());
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

### 🧾 DIAN Electronic Billing — Health Services
```java
// Billing Service — DIAN integration for medical services
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
            // 1. Generate UBL 2.1 XML for health services
            String xmlContent = generateHealthInvoiceXML(invoice);

            // 2. Calculate CUFE
            String cufe = calculateCUFE(invoice);
            invoice.setCufe(cufe);

            // 3. Sign with digital certificate
            String signedXml = signatureService.signXML(xmlContent);

            // 4. Submit to DIAN
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

                log.info("Medical invoice {} approved by DIAN", invoice.getNumber());
            } else {
                invoice.setDianStatus(DianStatus.REJECTED);
                if (dianResponse != null) {
                    invoice.setRejectionReason(dianResponse.getErrors());
                }
                log.error("Medical invoice {} rejected by DIAN", invoice.getNumber());
            }

            invoiceRepository.save(invoice);
            return dianResponse;

        } catch (Exception e) {
            log.error("DIAN submission error for invoice {}", invoice.getNumber(), e);
            throw new BillingServiceException("DIAN integration failed", e);
        }
    }

    private String calculateCUFE(MedicalInvoice invoice) {
        // CUFE = SHA-384(Num + Date + Time + SubTotal + Tax + Total 
        //               + NitIssuer + DocAcq + TechKey + Environment)
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

### 🧪 TDD/BDD — Testing Strategy
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
    @DisplayName("Should return patient hits when valid search term is provided")
    void shouldReturnPatientHitsWhenValidSearchTerm() {
        // Given
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

        // When
        SearchHits<PatientDocument> result =
            clinicalSearchService.searchPatients(request);

        // Then
        assertThat(result.getTotalHits()).isEqualTo(1L);
        assertThat(result.getSearchHit(0).getContent().getFullName())
            .isEqualTo("Maria Garcia Lopez");

        verify(elasticsearchOperations, times(1))
            .search(any(NativeSearchQuery.class), eq(PatientDocument.class));
    }

    @Test
    @DisplayName("Should index patient document when Kafka event is received")
    void shouldIndexPatientWhenKafkaEventReceived() {
        // Given
        PatientIndexEvent event = PatientIndexEvent.builder()
            .patientId(UUID.randomUUID())
            .fullName("Carlos Perez")
            .documentNumber("98765432")
            .epsCode("SURA-001")
            .lastVisit(LocalDate.now())
            .build();

        // When
        clinicalSearchService.indexPatientEvent(event);

        // Then
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

describe('ClinicalSearch Component', () => {
  beforeEach(() => {
    jest.clearAllMocks();
  });

  it('should render search input and button', () => {
    renderWithStore();
    expect(screen.getByTestId('clinical-search-input')).toBeInTheDocument();
    expect(screen.getByTestId('search-button')).toBeInTheDocument();
  });

  it('should dispatch searchPatients thunk on button click', async () => {
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

  it('should display error message when search fails', async () => {
    (searchService.searchPatients as jest.Mock).mockRejectedValue({
      response: { data: { message: 'Search service unavailable' } },
    });

    renderWithStore();

    fireEvent.change(screen.getByTestId('clinical-search-input'), {
      target: { value: 'test query' },
    });
    fireEvent.click(screen.getByTestId('search-button'));

    await waitFor(() => {
      expect(screen.getByTestId('search-error')).toHaveTextContent(
        'Search service unavailable'
      );
    });
  });
});
```
```javascript
// Cypress BDD — E2E Clinical Search Flow
describe('Clinical Search - BDD', () => {
  beforeEach(() => {
    cy.login('physician@clinicacore.com', 'testpass');
    cy.visit('/dashboard/search');
  });

  it('Given a physician, When searching a patient by name, Then results should appear', () => {
    // Given
    cy.intercept('GET', '/api/search/patients*', {
      fixture: 'search-results.json',
    }).as('searchRequest');

    // When
    cy.get('[data-testid="clinical-search-input"]').type('Maria Garcia');
    cy.get('[data-testid="search-button"]').click();

    // Then
    cy.wait('@searchRequest');
    cy.get('[data-testid="patient-result-card"]').should('have.length.greaterThan', 0);
    cy.get('[data-testid="patient-result-card"]').first()
      .should('contain.text', 'Maria Garcia');
  });

  it('Given a physician, When typing a diagnosis prefix, Then ICD-10 suggestions appear', () => {
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

## 📊 Data Model

### Primary Schema — PostgreSQL
```sql
-- Patients
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

-- Physicians
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

-- Appointments
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

-- Electronic Health Records (Historia Clínica Electrónica — Res. 1995/1999)
CREATE TABLE medical_records (
    id                  BIGSERIAL PRIMARY KEY,
    patient_id          BIGINT REFERENCES patients(id) NOT NULL,
    appointment_id      BIGINT REFERENCES appointments(id),
    physician_id        BIGINT REFERENCES physicians(id) NOT NULL,
    record_date         TIMESTAMP NOT NULL,
    version             INTEGER NOT NULL DEFAULT 1,    -- Immutable versioning
    is_current          BOOLEAN DEFAULT TRUE,
    reason_for_visit    TEXT NOT NULL,
    current_illness     TEXT,
    vital_signs         JSONB,                         -- HR, BP, Temp, Weight, Height
    physical_exam       TEXT,
    clinical_impression TEXT,
    treatment_plan      TEXT,
    observations        TEXT,
    created_at          TIMESTAMP DEFAULT CURRENT_TIMESTAMP
    -- No UPDATE allowed — new version created on every change
);

-- Diagnoses (ICD-10)
CREATE TABLE diagnoses (
    id                  BIGSERIAL PRIMARY KEY,
    record_id           BIGINT REFERENCES medical_records(id) NOT NULL,
    icd10_code          VARCHAR(10) NOT NULL,
    icd10_description   VARCHAR(500) NOT NULL,
    diagnosis_type      VARCHAR(20) NOT NULL,          -- PRINCIPAL, RELATED, COMPLICATION
    diagnosis_status    VARCHAR(20) NOT NULL,          -- CONFIRMED, SUSPECTED, RULED_OUT
    created_at          TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

-- Prescriptions
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

-- Prescription Items
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

-- Medical Invoices
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

    -- DIAN Fields
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

-- Invoice Line Items
CREATE TABLE invoice_items (
    id                  BIGSERIAL PRIMARY KEY,
    invoice_id          BIGINT REFERENCES medical_invoices(id) ON DELETE CASCADE,
    service_code        VARCHAR(50),                   -- CUPS code (Colombian health)
    service_description VARCHAR(500) NOT NULL,
    quantity            INTEGER NOT NULL DEFAULT 1,
    unit_price          DECIMAL(10,2) NOT NULL,
    tax_rate            DECIMAL(5,2) DEFAULT 0,
    tax_amount          DECIMAL(10,2) DEFAULT 0,
    subtotal            DECIMAL(10,2) NOT NULL
);

-- ElasticSearch Sync Control
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

-- Audit Log
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

-- Indexes for performance
CREATE INDEX idx_patients_document ON patients(document_number);
CREATE INDEX idx_patients_eps ON patients(eps_code);
CREATE INDEX idx_appointments_date ON appointments(appointment_date, physician_id);
CREATE INDEX idx_appointments_patient ON appointments(patient_id, status);
CREATE INDEX idx_records_patient ON medical_records(patient_id, is_current);
CREATE INDEX idx_diagnoses_icd10 ON diagnoses(icd10_code);
CREATE INDEX idx_invoices_dian_status ON medical_invoices(dian_status);
CREATE INDEX idx_audit_entity ON audit_log(entity_type, entity_id);
```

### NoSQL Schema — MongoDB (Clinical Logs & Unstructured Data)
```javascript
// Clinical Event Log — MongoDB Collection
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

// ElasticSearch Raw Query Log — MongoDB Collection
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

## 🎓 Key Learnings

### Architecture and Backend

✅ **Microservices with Java 11** — Designed and implemented distributed services
using Spring Boot 2.7.x and Spring MVC 5.3.x, maintaining compatibility with
Java 11 LTS, demonstrating range across Java ecosystem versions

✅ **ElasticSearch 7.x** — Full-text clinical search with custom analyzers,
multi-field boosting, fuzzy matching, ICD-10 autocomplete, and Index Lifecycle
Management (ILM) for audit data retention policies

✅ **Event-Driven Indexing** — Kafka-driven index synchronization ensuring
ElasticSearch always reflects the latest state across all microservices without
coupling service boundaries

✅ **Spring Cloud** — API Gateway with JWT validation, Eureka service discovery,
Config Server for centralized environment management, and Resilience4j for
circuit breaking under high patient load scenarios

✅ **Immutable Medical Records** — Implemented EHR versioning strategy compliant
with Resolución 1995/1999: no record is ever overwritten, every clinical change
generates a new immutable version with complete audit trail

✅ **TDD with JUnit 5 + Mockito** — Test-driven development across all service
layers, achieving high coverage on business-critical paths including DIAN
integration, ElasticSearch queries, and Kafka event processing

### Frontend and UX

✅ **React 18 + TypeScript** — Built type-safe SPA with functional components,
custom hooks, and strict TypeScript configuration for early error detection

✅ **Redux Toolkit + Thunks** — Implemented centralized state management with
Redux Toolkit slices and Redux Thunk middleware for all async operations,
replacing legacy connect() patterns with hooks-based useSelector/useDispatch

✅ **Webpack 5 Manual Configuration** — Configured production-grade Webpack
bundle manually: code splitting by route, tree shaking, lazy loading, SASS
compilation, asset optimization, and environment-specific builds without
Create React App abstractions

✅ **SASS Architecture** — Structured SASS codebase following 7-1 pattern:
global variables, mixins, typography, layout, and component-scoped modules
for consistent design system across 20+ clinical UI components

✅ **React Testing Library + Cypress** — Applied TDD on components using
Testing Library's user-centric approach, and BDD E2E scenarios with Cypress
covering critical clinical workflows: patient search, appointment booking,
and record access flows

### Integrations and Compliance

✅ **DIAN Electronic Billing** — Integrated Colombian electronic invoicing
for healthcare services with UBL 2.1 XML generation, CUFE calculation,
PKCS#12 digital signature, and real-time DIAN validation for medical
service billing codes (CUPS)

✅ **RIPS Generation** — Automated mandatory RIPS reports (Registro Individual
de Prestación de Servicios) required by MinSalud for EPS reimbursement,
aggregating data across four microservices with strict format validation

✅ **Colombian Health Regulations** — Deep implementation of Resolución
1995/1999 (electronic health records), ICD-10 diagnostic coding, CUPS
procedure coding, and ADRES beneficiary validation

### DevOps and Operations

✅ **CI/CD with Jenkins** — Configured declarative pipelines covering:
build, unit test, integration test, Docker image build, registry push,
and Kubernetes deployment with rollback on failure

✅ **ELK Stack** — Full observability stack: Logstash pipelines for
structured clinical event ingestion, ElasticSearch as both search engine
and log store, Kibana dashboards for operational monitoring and
search analytics

✅ **Kubernetes Production** — Managed multi-service Kubernetes deployments
with ConfigMaps, Secrets, horizontal pod autoscaling, liveness and
readiness probes, and zero-downtime rolling updates

---

## 🔄 Roadmap and Future Improvements

### Phase 2 — Q2 2025
- Mobile app for physicians (React Native) with offline EHR access
- AI-assisted diagnosis suggestion based on symptoms (NLP + ElasticSearch MLkit)
- Telemedicine module with WebRTC video consultation
- WhatsApp appointment reminders via Twilio

### Phase 3 — Q3 2025
- Multi-tenancy support for clinic networks and hospital groups
- Integration with national PISIS system (MinSalud epidemiological reporting)
- Automated ICD-10 coding suggestions from clinical notes using ML
- Electronic prescription with pharmacy chain integration

### Phase 4 — Q4 2025
- Predictive analytics dashboard for patient no-show rates
- IoT integration for vital signs devices (glucometers, oximeters)
- Cross-clinic patient data portability with patient consent
- FHIR R4 API for interoperability with external health systems

---

## 📈 Impact and Results

This system has been implemented and validated in **Colombian private medical
offices and clinics** through SENA, generating measurable operational improvements:

✅ **85% reduction** in patient lookup time using ElasticSearch vs traditional SQL search
✅ **95%+ DIAN approval rate** on first submission for medical service invoices
✅ **100% regulatory compliance** with Resolución 1995/1999 electronic health record requirements
✅ **Zero manual RIPS errors** — automated generation eliminated transcription mistakes
✅ **60% reduction** in appointment scheduling time through real-time calendar management
✅ **Full audit trail** on every clinical record access, meeting HABEAS DATA requirements

---

## 📄 License and Intellectual Property

> ⚠️ **SENA — Intellectual Property Disclaimer**
>
> This project was developed as part of academic and research activities within
> **SENA (Servicio Nacional de Aprendizaje)** under the **SENNOVA** program,
> aimed at supporting digital transformation in Colombian healthcare micro-enterprises.
>
> The **source code, architecture design, and technical documentation are
> institutional property of SENA** and are not publicly available in this repository.
> The content presented here — including technical specifications, architecture
> diagrams, code samples, and data models — has been **recreated for portfolio
> demonstration purposes only**, without exposing confidential institutional
> information or the original production codebase.
>
> Screenshots and UI captures have been intentionally excluded to protect
> patient data privacy and institutional confidentiality.

**Available for:**

✅ Custom consulting and implementation for private clinics
✅ DIAN electronic billing training for healthcare providers
✅ ElasticSearch search architecture design and optimization
✅ Colombian health regulation compliance advisory (Res. 1995/1999, RIPS)
✅ Additional module development and technical support

---

## 🏆 Recognition

- 🥇 **Best Healthcare Innovation Project** — SENA Regional Risaralda 2024
- 🏅 **DIAN Certification** — Validated system for electronic health service invoicing
- 🏥 **MinSalud Alignment** — Architecture validated against Resolución 1995/1999
- ⭐ **Implemented in 10+ Colombian private medical offices**

---

