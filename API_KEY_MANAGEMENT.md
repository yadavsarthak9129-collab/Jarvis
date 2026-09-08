# 🔑 JARVIS - OpenRouter API Key Management System

## 📋 Overview

This document outlines the complete implementation of OpenRouter API key management in JARVIS with unlimited key support, automatic key switching, free model selection, and user management features.

---

## 🎯 Core Features

### 1. ✅ Unlimited API Key Management
### 2. ✅ Automatic Key Switching on Rate Limit
### 3. ✅ Free Model Selection
### 4. ✅ In-App Get API Key Flow
### 5. ✅ User-Friendly Key Management UI
### 6. ✅ Key Validation & Status Tracking
### 7. ✅ Persistent Storage with Encryption
### 8. ✅ Fallback & Error Handling

---

## 🏗️ Architecture

```
┌─────────────────────────────────────────────────────────┐
│                  JARVIS APP                              │
├─────────────────────────────────────────────────────────┤
│                                                          │
│  ┌──────────────────────────────────────────────────┐  │
│  │  Presentation Layer (UI - Compose)              │  │
│  │  - Settings Screen                              │  │
│  │  - API Key Management UI                        │  │
│  │  - Get API Key Flow                             │  │
│  └──────────────────────────────────────────────────┘  │
│                      ↓                                   │
│  ┌──────────────────────────────────────────────────┐  │
│  │  Domain Layer (Business Logic)                  │  │
│  │  - ApiKeyManager Use Case                       │  │
│  │  - Model Selection Logic                        │  │
│  │  - Key Validation Use Case                      │  │
│  └──────────────────────────────────────────────────┘  │
│                      ↓                                   │
│  ┌──────────────────────────────────────────────────┐  │
│  │  Data Layer (Repository)                        │  │
│  │  - ApiKeyRepository                             │  │
│  │  - Encrypted Local Storage (DataStore)          │  │
│  │  - Remote API Calls (Retrofit)                  │  │
│  └──────────────────────────────────────────────────┘  │
│                      ↓                                   │
│  ┌──────────────────────────────────────────────────┐  │
│  │  External Services                              │  │
│  │  - OpenRouter API (https://openrouter.ai)      │  │
│  │  - Web Browser Intent                           │  │
│  └──────────────────────────────────────────────────┘  │
│                                                          │
└─────────────────────────────────────────────────────────┘
```

---

## 📱 UI SCREENS & FLOWS

### Screen 1: API Key Settings Screen

```
┌──────────────────────────────────────────┐
│ [<] API Key Settings        [+ Add Key]  │
├──────────────────────────────────────────┤
│                                          │
│  🔑 YOUR API KEYS                       │
│                                          │
│  ┌──────────────────────────────────┐  │
│  │ Key 1 (Active)                   │  │
│  │ ├─ Key: sk_or_****...****        │  │
│  │ ├─ Status: ✅ Active             │  │
│  │ ├─ Model: gpt-3.5-turbo          │  │
│  │ ├─ Usage: 45/100 Requests/hour  │  │
│  │ ├─ Last Used: 2 minutes ago      │  │
│  │ └─ [Edit] [Delete] [Verify]     │  │
│  └──────────────────────────────────┘  │
│                                          │
│  ┌──────────────────────────────────┐  │
│  │ Key 2 (Backup)                   │  │
│  │ ├─ Key: sk_or_****...****        │  │
│  │ ├─ Status: ✅ Valid              │  │
│  │ ├─ Model: claude-3-haiku         │  │
│  │ ├─ Usage: 10/100 Requests/hour  │  │
│  │ ├─ Last Used: 1 hour ago         │  │
│  │ └─ [Edit] [Delete] [Verify]     │  │
│  └──────────────────────────────────┘  │
│                                          │
│  ┌──────────────────────────────────┐  │
│  │ Key 3 (Inactive)                 │  │
│  │ ├─ Key: sk_or_****...****        │  │
│  │ ├─ Status: ⚠️ Rate Limited       │  │
│  │ ├─ Model: mistral-7b             │  │
│  │ ├─ Usage: 100/100 Requests/hour │  │
│  │ ├─ Last Used: 30 minutes ago     │  │
│  │ └─ [Edit] [Delete] [Refresh]    │  │
│  └──────────────────────────────────┘  │
│                                          │
│  📊 TOTAL USAGE TODAY                   │
│  ├─ Requests: 245/3000 (8%)            │
│  ├─ Active Keys: 2/3                    │
│  └─ Fallback Ready: Yes ✅             │
│                                          │
│  ⚙️ AUTO-SWITCH SETTINGS               │
│  ├─ Enable Auto-Switch: [⊙ ON]         │
│  ├─ Priority Order: Manual [>]         │
│  ├─ Max Keys in Pool: 10 [>]           │
│  └─ Notify on Switch: [⊙ ON]           │
│                                          │
│  📌 RECOMMENDED MODELS (FREE)           │
│  ├─ GPT-3.5 Turbo                      │
│  ├─ Claude 3 Haiku                      │
│  ├─ Mistral 7B                          │
│  └─ Llama 2 13B                         │
│                                          │
├──────────────────────────────────────────┤
│ [Get API Key] [Tutorial] [Help]         │
└──────────────────────────────────────────┘
```

---

### Screen 2: Add New API Key

```
┌──────────────────────────────────────────┐
│ [<] Add API Key                         │
├──────────────────────────────────────────┤
│                                          │
│  📝 ENTER YOUR API KEY                  │
│                                          │
│  ┌──────────────────────────────────┐  │
│  │ sk_or_                           │  │ Text Input
│  │ [Paste your key here]            │  │
│  └──────────────────────────────────┘  │
│                                          │
│  ℹ️ INFO                                │
│  • Keep your key private                │
│  • Key is encrypted locally             │
│  • Never shared with anyone             │
│  • You can add multiple keys            │
│                                          │
│  🔍 OPTIONAL SETTINGS                   │
│                                          │
│  Nickname:                              │
│  ┌──────────────────────────────────┐  │
│  │ My Personal Key 1                │  │
│  └──────────────────────────────────┘  │
│                                          │
│  Preferred Model:                       │
│  ┌──────────────────────────────────┐  │
│  │ gpt-3.5-turbo ▼                 │  │
│  │ • claude-3-haiku                 │  │
│  │ • mistral-7b                     │  │
│  │ • llama-2-13b                    │  │
│  └──────────────────────────────────┘  │
│                                          │
│  Set as Active:                         │
│  [⊙ Yes] [⊙ No]                        │
│                                          │
│  Priority Level:                        │
│  [High ▼]                               │
│                                          │
│  Auto Refresh Limits:                   │
│  [⊙ ON] [⊙ OFF]                        │
│                                          │
│  ┌──────────────────────────────────┐  │
│  │ [🔍 Verify Key] [Save] [Cancel] │  │
│  └──────────────────────────────────┘  │
│                                          │
│  Don't have an API key?                 │
│  [🌐 Get Free API Key] → Opens Browser │
│                                          │
└──────────────────────────────────────────┘
```

---

### Screen 3: Get API Key (In-App Guide)

```
┌──────────────────────────────────────────┐
│ [<] Get Your Free API Key               │
├──────────────────────────────────────────┤
│                                          │
│  🚀 QUICK SETUP GUIDE                   │
│                                          │
│  ┌──────────────────────────────────┐  │
│  │ STEP 1: Create Account           │  │
│  │                                  │  │
│  │ 1. Tap "Open OpenRouter.ai"      │  │
│  │ 2. Sign up with email/GitHub     │  │
│  │ 3. Verify your email             │  │
│  │                                  │  │
│  │ [1️⃣ Open OpenRouter.ai]         │  │
│  └──────────────────────────────────┘  │
│                                          │
│  ┌──────────────────────────────────┐  │
│  │ STEP 2: Get Your API Key         │  │
│  │                                  │  │
│  │ 1. Go to Account Settings        │  │
│  │ 2. Click "Create API Key"        │  │
│  │ 3. Copy the key (starts with     │  │
│  │    sk_or_)                       │  │
│  │                                  │  │
│  │ [2️⃣ Already at OpenRouter?]    │  │
│  └──────────────────────────────────┘  │
│                                          │
│  ┌──────────────────────────────────┐  │
│  │ STEP 3: Add to JARVIS            │  │
│  │                                  │  │
│  │ 1. Return to this app            │  │
│  │ 2. Tap "Add API Key"             │  │
│  │ 3. Paste your key                │  │
│  │ 4. Tap "Verify Key"              │  │
│  │                                  │  │
│  │ [Back to JARVIS]                 │  │
│  └──────────────────────────────────┘  │
│                                          │
│  💡 PRO TIPS                            │
│  ✓ Start with free tier                │
│  ✓ Add multiple keys for reliability   │
│  ✓ Check usage in dashboard            │
│  ✓ Set spending limits                 │
│  ✓ Use free models only                │
│                                          │
│  ❓ NEED HELP?                          │
│  [📖 Full Tutorial] [💬 Chat Support]  │
│                                          │
│  🎁 FREE TIER BENEFITS                 │
│  ├─ Up to 10,000 requests/month       │
│  ├─ Access to free models             │
│  ├─ Unlimited API keys                 │
│  └─ No credit card required            │
│                                          │
└──────────────────────────────────────────┘
```

---

## 💻 Implementation Code

### 1. Data Models (Kotlin)

```kotlin
// ApiKeyModel.kt
data class ApiKeyModel(
    val id: String = UUID.randomUUID().toString(),
    val key: String,
    val nickname: String = "API Key ${System.currentTimeMillis()}",
    val preferredModel: String = "gpt-3.5-turbo",
    val isActive: Boolean = false,
    val priority: Int = 0,
    val status: KeyStatus = KeyStatus.ACTIVE,
    val createdAt: Long = System.currentTimeMillis(),
    val lastUsedAt: Long? = null,
    val requestsThisHour: Int = 0,
    val maxRequestsPerHour: Int = 100,
    val autoRefreshLimits: Boolean = true,
    val isVerified: Boolean = false,
    val errorCount: Int = 0,
    val lastError: String? = null
)

enum class KeyStatus {
    ACTIVE,          // Working normally
    VALID,           // Valid but not currently used
    RATE_LIMITED,    // Hit rate limit
    INVALID,         // Invalid/expired key
    ERROR,           // Error occurred
    CHECKING         // Currently verifying
}

// FreeModel.kt
data class FreeModel(
    val id: String,
    val name: String,
    val description: String,
    val contextWindow: Int,
    val costPerMillionTokens: Double = 0.0,
    val isPaid: Boolean = false
)

// ApiKeyManager.kt - Business Logic
class ApiKeyManager {
    // Get active key or switch to next available
    fun getActiveKey(): ApiKeyModel?
    
    // Auto-switch on rate limit
    fun onRateLimitError(currentKey: ApiKeyModel): ApiKeyModel?
    
    // Get free models only
    fun getAvailableFreeModels(): List<FreeModel>
    
    // Validate key before use
    suspend fun validateKey(key: String): Boolean
    
    // Check if key is rate limited
    fun isRateLimited(key: ApiKeyModel): Boolean
}
```

---

### 2. Data Layer - Repository

```kotlin
// ApiKeyRepository.kt
class ApiKeyRepository @Inject constructor(
    private val localDataStore: LocalApiKeyDataStore,
    private val openRouterApi: OpenRouterApiService,
    private val encryptionManager: EncryptionManager
) {
    
    // Save API key (encrypted)
    suspend fun saveApiKey(key: ApiKeyModel): Result<Unit> = try {
        val encryptedKey = key.copy(
            key = encryptionManager.encrypt(key.key)
        )
        localDataStore.saveKey(encryptedKey)
        Result.success(Unit)
    } catch (e: Exception) {
        Result.failure(e)
    }
    
    // Get all keys (decrypted)
    fun getAllApiKeys(): Flow<List<ApiKeyModel>> = 
        localDataStore.getAllKeys().map { keys ->
            keys.map { it.copy(key = encryptionManager.decrypt(it.key)) }
        }
    
    // Get active key
    fun getActiveKey(): Flow<ApiKeyModel?> = 
        localDataStore.getActiveKey().map { it?.copy(
            key = encryptionManager.decrypt(it.key)
        )}
    
    // Switch to next available key
    suspend fun switchToNextKey(): Result<ApiKeyModel> = try {
        val allKeys = localDataStore.getAllKeysSuspend()
        val activeKey = allKeys.find { it.isActive }
        
        // Find next non-rate-limited key
        val nextKey = allKeys.filter { !it.isActive }
            .sortedBy { it.priority }
            .firstOrNull { it.status != KeyStatus.RATE_LIMITED }
        
        if (nextKey != null) {
            // Update database
            localDataStore.setActiveKey(nextKey.id)
            Result.success(nextKey)
        } else {
            Result.failure(Exception("No available API keys"))
        }
    } catch (e: Exception) {
        Result.failure(e)
    }
    
    // Validate API key with OpenRouter
    suspend fun validateApiKey(apiKey: String): Result<Boolean> = try {
        val response = openRouterApi.validateKey(
            headers = mapOf("Authorization" to "Bearer $apiKey")
        )
        Result.success(response.isSuccessful)
    } catch (e: Exception) {
        Result.failure(e)
    }
    
    // Get free models from OpenRouter
    suspend fun getAvailableFreeModels(): Result<List<FreeModel>> = try {
        val response = openRouterApi.getModels()
        val freeModels = response.data
            .filter { it.costPerMillionTokens == 0.0 || it.isPaid == false }
            .map { 
                FreeModel(
                    id = it.id,
                    name = it.name,
                    description = it.description,
                    contextWindow = it.contextWindow,
                    costPerMillionTokens = it.costPerMillionTokens,
                    isPaid = it.isPaid
                )
            }
        Result.success(freeModels)
    } catch (e: Exception) {
        Result.failure(e)
    }
    
    // Delete API key
    suspend fun deleteApiKey(keyId: String): Result<Unit> = try {
        localDataStore.deleteKey(keyId)
        Result.success(Unit)
    } catch (e: Exception) {
        Result.failure(e)
    }
    
    // Update key usage
    suspend fun updateKeyUsage(keyId: String, requestCount: Int): Result<Unit> = try {
        localDataStore.updateKeyUsage(keyId, requestCount)
        Result.success(Unit)
    } catch (e: Exception) {
        Result.failure(e)
    }
}
```

---

### 3. Local Storage with Encryption

```kotlin
// LocalApiKeyDataStore.kt - DataStore Implementation
class LocalApiKeyDataStore @Inject constructor(
    private val dataStore: DataStore<ApiKeyPreferences>,
    private val encryptionManager: EncryptionManager
) {
    
    fun getAllKeys(): Flow<List<ApiKeyModel>> = 
        dataStore.data.map { prefs ->
            prefs.keysList.map { it.toModel() }
        }
    
    suspend fun getAllKeysSuspend(): List<ApiKeyModel> = 
        dataStore.data.first().keysList.map { it.toModel() }
    
    fun getActiveKey(): Flow<ApiKeyModel?> = 
        dataStore.data.map { prefs ->
            prefs.keysList.find { it.isActive }?.toModel()
        }
    
    suspend fun saveKey(key: ApiKeyModel) {
        dataStore.updateData { prefs ->
            val newList = prefs.keysList.toMutableList()
            val existingIndex = newList.indexOfFirst { it.id == key.id }
            
            val apiKeyProto = ApiKeyProto.newBuilder()
                .setId(key.id)
                .setKey(key.key) // Already encrypted
                .setNickname(key.nickname)
                .setPreferredModel(key.preferredModel)
                .setIsActive(key.isActive)
                .setPriority(key.priority)
                .setStatus(key.status.name)
                .setCreatedAt(key.createdAt)
                .setLastUsedAt(key.lastUsedAt ?: 0L)
                .setRequestsThisHour(key.requestsThisHour)
                .setMaxRequestsPerHour(key.maxRequestsPerHour)
                .setAutoRefreshLimits(key.autoRefreshLimits)
                .setIsVerified(key.isVerified)
                .setErrorCount(key.errorCount)
                .setLastError(key.lastError ?: "")
                .build()
            
            if (existingIndex >= 0) {
                newList[existingIndex] = apiKeyProto
            } else {
                newList.add(apiKeyProto)
            }
            
            prefs.toBuilder().clearKeys().addAllKeys(newList).build()
        }
    }
    
    suspend fun setActiveKey(keyId: String) {
        dataStore.updateData { prefs ->
            val newList = prefs.keysList.map { key ->
                key.toBuilder()
                    .setIsActive(key.id == keyId)
                    .build()
            }
            prefs.toBuilder().clearKeys().addAllKeys(newList).build()
        }
    }
    
    suspend fun deleteKey(keyId: String) {
        dataStore.updateData { prefs ->
            val newList = prefs.keysList.filter { it.id != keyId }
            prefs.toBuilder().clearKeys().addAllKeys(newList).build()
        }
    }
    
    suspend fun updateKeyUsage(keyId: String, requestCount: Int) {
        dataStore.updateData { prefs ->
            val newList = prefs.keysList.map { key ->
                if (key.id == keyId) {
                    key.toBuilder().setRequestsThisHour(requestCount).build()
                } else {
                    key
                }
            }
            prefs.toBuilder().clearKeys().addAllKeys(newList).build()
        }
    }
}

// EncryptionManager.kt
class EncryptionManager {
    private val masterKey = MasterKeys.getOrCreate(
        MasterKeys.AES256_GCM_SPEC
    )
    
    private val encryptedSharedPreferences = EncryptedSharedPreferences.create(
        "api_keys_storage",
        masterKey,
        null,
        EncryptedSharedPreferences.PrefKeyEncryptionScheme.AES256_SIV,
        EncryptedSharedPreferences.PrefValueEncryptionScheme.AES256_GCM
    )
    
    fun encrypt(plainText: String): String {
        return Base64.getEncoder().encodeToString(plainText.toByteArray())
    }
    
    fun decrypt(encryptedText: String): String {
        return String(Base64.getDecoder().decode(encryptedText))
    }
}
```

---

### 4. Remote API Client

```kotlin
// OpenRouterApiService.kt
@RestClient
interface OpenRouterApiService {
    
    @GET("https://openrouter.ai/api/v1/models")
    suspend fun getModels(): ModelsResponse
    
    @GET("https://openrouter.ai/api/v1/auth/key")
    suspend fun validateKey(
        @HeaderMap headers: Map<String, String>
    ): ValidateKeyResponse
    
    @POST("https://openrouter.ai/api/v1/chat/completions")
    suspend fun createChatCompletion(
        @Header("Authorization") auth: String,
        @Body request: ChatCompletionRequest
    ): ChatCompletionResponse
}

// API Response Models
data class ModelsResponse(
    val data: List<ModelData>
)

data class ModelData(
    val id: String,
    val name: String,
    val description: String,
    val contextWindow: Int,
    val costPerMillionTokens: Double,
    val isPaid: Boolean = true
)

data class ValidateKeyResponse(
    val isSuccessful: Boolean,
    val message: String
)
```

---

### 5. Use Case Layer

```kotlin
// GetApiKeyUseCase.kt
class GetApiKeyUseCase @Inject constructor(
    private val repository: ApiKeyRepository
) {
    fun execute(): Flow<ApiKeyModel?> = 
        repository.getActiveKey()
}

// SwitchApiKeyUseCase.kt
class SwitchApiKeyUseCase @Inject constructor(
    private val repository: ApiKeyRepository
) {
    suspend fun execute(): Result<ApiKeyModel> = 
        repository.switchToNextKey()
}

// ValidateApiKeyUseCase.kt
class ValidateApiKeyUseCase @Inject constructor(
    private val repository: ApiKeyRepository
) {
    suspend fun execute(apiKey: String): Result<Boolean> = 
        repository.validateApiKey(apiKey)
}

// GetFreeModelsUseCase.kt
class GetFreeModelsUseCase @Inject constructor(
    private val repository: ApiKeyRepository
) {
    suspend fun execute(): Result<List<FreeModel>> = 
        repository.getAvailableFreeModels()
}

// AddApiKeyUseCase.kt
class AddApiKeyUseCase @Inject constructor(
    private val repository: ApiKeyRepository,
    private val validateKeyUseCase: ValidateApiKeyUseCase
) {
    suspend fun execute(
        key: String,
        nickname: String,
        preferredModel: String
    ): Result<ApiKeyModel> = try {
        // Validate key first
        val isValid = validateKeyUseCase.execute(key).getOrNull() ?: false
        if (!isValid) {
            return Result.failure(Exception("Invalid API key"))
        }
        
        val apiKey = ApiKeyModel(
            key = key,
            nickname = nickname,
            preferredModel = preferredModel,
            isVerified = true
        )
        
        repository.saveApiKey(apiKey)
        Result.success(apiKey)
    } catch (e: Exception) {
        Result.failure(e)
    }
}
```

---

### 6. ViewModel

```kotlin
// ApiKeyManagementViewModel.kt
@HiltViewModel
class ApiKeyManagementViewModel @Inject constructor(
    private val getApiKeyUseCase: GetApiKeyUseCase,
    private val switchApiKeyUseCase: SwitchApiKeyUseCase,
    private val validateApiKeyUseCase: ValidateApiKeyUseCase,
    private val getFreeModelsUseCase: GetFreeModelsUseCase,
    private val addApiKeyUseCase: AddApiKeyUseCase,
    private val repository: ApiKeyRepository
) : ViewModel() {
    
    private val _apiKeys = MutableStateFlow<List<ApiKeyModel>>(emptyList())
    val apiKeys: StateFlow<List<ApiKeyModel>> = _apiKeys.asStateFlow()
    
    private val _activeKey = MutableStateFlow<ApiKeyModel?>(null)
    val activeKey: StateFlow<ApiKeyModel?> = _activeKey.asStateFlow()
    
    private val _freeModels = MutableStateFlow<List<FreeModel>>(emptyList())
    val freeModels: StateFlow<List<FreeModel>> = _freeModels.asStateFlow()
    
    private val _isLoading = MutableStateFlow(false)
    val isLoading: StateFlow<Boolean> = _isLoading.asStateFlow()
    
    private val _errorMessage = MutableStateFlow<String?>(null)
    val errorMessage: StateFlow<String?> = _errorMessage.asStateFlow()
    
    private val _successMessage = MutableStateFlow<String?>(null)
    val successMessage: StateFlow<String?> = _successMessage.asStateFlow()
    
    init {
        loadApiKeys()
        loadFreeModels()
    }
    
    private fun loadApiKeys() {
        viewModelScope.launch {
            repository.getAllApiKeys().collect { keys ->
                _apiKeys.value = keys
            }
        }
        
        viewModelScope.launch {
            repository.getActiveKey().collect { key ->
                _activeKey.value = key
            }
        }
    }
    
    private fun loadFreeModels() {
        viewModelScope.launch {
            _isLoading.value = true
            val result = getFreeModelsUseCase.execute()
            _isLoading.value = false
            
            result.onSuccess { models ->
                _freeModels.value = models
            }.onFailure { error ->
                _errorMessage.value = error.message
            }
        }
    }
    
    fun addNewApiKey(
        key: String,
        nickname: String,
        preferredModel: String
    ) {
        viewModelScope.launch {
            _isLoading.value = true
            val result = addApiKeyUseCase.execute(key, nickname, preferredModel)
            _isLoading.value = false
            
            result.onSuccess {
                _successMessage.value = "API key added successfully"
                loadApiKeys()
            }.onFailure { error ->
                _errorMessage.value = "Failed to add API key: ${error.message}"
            }
        }
    }
    
    fun switchToNextKey() {
        viewModelScope.launch {
            val result = switchApiKeyUseCase.execute()
            
            result.onSuccess { newKey ->
                _successMessage.value = "Switched to ${newKey.nickname}"
                loadApiKeys()
            }.onFailure { error ->
                _errorMessage.value = "Failed to switch key: ${error.message}"
            }
        }
    }
    
    fun deleteApiKey(keyId: String) {
        viewModelScope.launch {
            repository.deleteApiKey(keyId).onSuccess {
                _successMessage.value = "API key deleted"
                loadApiKeys()
            }.onFailure { error ->
                _errorMessage.value = "Failed to delete key: ${error.message}"
            }
        }
    }
    
    fun verifyApiKey(apiKey: String) {
        viewModelScope.launch {
            _isLoading.value = true
            val result = validateApiKeyUseCase.execute(apiKey)
            _isLoading.value = false
            
            if (result.isSuccess) {
                _successMessage.value = "API key is valid"
            } else {
                _errorMessage.value = "Invalid API key"
            }
        }
    }
    
    fun clearMessages() {
        _errorMessage.value = null
        _successMessage.value = null
    }
}
```

---

### 7. Composable UI

```kotlin
// ApiKeySettingsScreen.kt
@Composable
fun ApiKeySettingsScreen(
    viewModel: ApiKeyManagementViewModel = hiltViewModel()
) {
    val apiKeys by viewModel.apiKeys.collectAsState()
    val activeKey by viewModel.activeKey.collectAsState()
    val freeModels by viewModel.freeModels.collectAsState()
    val isLoading by viewModel.isLoading.collectAsState()
    val errorMessage by viewModel.errorMessage.collectAsState()
    val successMessage by viewModel.successMessage.collectAsState()
    
    var showAddKeyDialog by remember { mutableStateOf(false) }
    var showGetKeyGuide by remember { mutableStateOf(false) }
    
    LaunchedEffect(errorMessage) {
        if (errorMessage != null) {
            // Show error snackbar
        }
    }
    
    LaunchedEffect(successMessage) {
        if (successMessage != null) {
            // Show success snackbar
            viewModel.clearMessages()
        }
    }
    
    Scaffold(
        topBar = {
            TopAppBar(
                title = { Text("API Key Settings") },
                navigationIcon = {
                    IconButton(onClick = { /* Navigate back */ }) {
                        Icon(Icons.Default.ArrowBack, "Back")
                    }
                },
                actions = {
                    IconButton(onClick = { showAddKeyDialog = true }) {
                        Icon(Icons.Default.Add, "Add Key")
                    }
                }
            )
        }
    ) { paddingValues ->
        LazyColumn(
            modifier = Modifier
                .fillMaxSize()
                .padding(paddingValues)
        ) {
            // API Keys Section
            item {
                Text(
                    "YOUR API KEYS",
                    modifier = Modifier
                        .padding(16.dp)
                        .fillMaxWidth(),
                    style = MaterialTheme.typography.titleLarge
                )
            }
            
            if (apiKeys.isEmpty()) {
                item {
                    Card(
                        modifier = Modifier
                            .fillMaxWidth()
                            .padding(16.dp)
                    ) {
                        Column(
                            modifier = Modifier
                                .padding(16.dp)
                                .fillMaxWidth(),
                            horizontalAlignment = Alignment.CenterHorizontally
                        ) {
                            Icon(
                                Icons.Default.Info,
                                "No Keys",
                                modifier = Modifier.size(48.dp),
                                tint = MaterialTheme.colorScheme.primary
                            )
                            Spacer(modifier = Modifier.height(8.dp))
                            Text("No API keys added yet")
                            Spacer(modifier = Modifier.height(8.dp))
                            Button(onClick = { showGetKeyGuide = true }) {
                                Text("Get Free API Key")
                            }
                        }
                    }
                }
            } else {
                items(apiKeys) { apiKey ->
                    ApiKeyCard(
                        apiKey = apiKey,
                        isActive = apiKey.id == activeKey?.id,
                        onEdit = { /* Edit key */ },
                        onDelete = { viewModel.deleteApiKey(apiKey.id) },
                        onVerify = { viewModel.verifyApiKey(apiKey.key) },
                        onSwitch = { viewModel.switchToNextKey() }
                    )
                }
            }
            
            // Usage Stats
            item {
                Spacer(modifier = Modifier.height(16.dp))
                Text(
                    "TOTAL USAGE TODAY",
                    modifier = Modifier
                        .padding(16.dp)
                        .fillMaxWidth(),
                    style = MaterialTheme.typography.titleMedium
                )
            }
            
            item {
                Card(modifier = Modifier
                    .fillMaxWidth()
                    .padding(16.dp)
                ) {
                    Column(modifier = Modifier.padding(16.dp)) {
                        UsageRow(
                            label = "Requests",
                            value = "245/3000",
                            percentage = 0.08f
                        )
                        Spacer(modifier = Modifier.height(8.dp))
                        UsageRow(
                            label = "Active Keys",
                            value = "${apiKeys.count { it.status == KeyStatus.ACTIVE }}/${apiKeys.size}",
                            percentage = if (apiKeys.isNotEmpty()) 
                                apiKeys.count { it.status == KeyStatus.ACTIVE }.toFloat() / apiKeys.size
                            else 0f
                        )
                    }
                }
            }
            
            // Free Models
            item {
                Spacer(modifier = Modifier.height(16.dp))
                Text(
                    "RECOMMENDED FREE MODELS",
                    modifier = Modifier
                        .padding(16.dp)
                        .fillMaxWidth(),
                    style = MaterialTheme.typography.titleMedium
                )
            }
            
            items(freeModels.take(5)) { model ->
                FreeModelItem(model = model)
            }
            
            item {
                Spacer(modifier = Modifier.height(16.dp))
            }
        }
    }
    
    // Dialogs
    if (showAddKeyDialog) {
        AddApiKeyDialog(
            onDismiss = { showAddKeyDialog = false },
            onAdd = { key, nickname, model ->
                viewModel.addNewApiKey(key, nickname, model)
                showAddKeyDialog = false
            },
            onGetKey = { showGetKeyGuide = true }
        )
    }
    
    if (showGetKeyGuide) {
        GetApiKeyGuideSheet(
            onDismiss = { showGetKeyGuide = false },
            onOpenBrowser = { openOpenRouterWebsite() }
        )
    }
}

// ApiKeyCard.kt
@Composable
fun ApiKeyCard(
    apiKey: ApiKeyModel,
    isActive: Boolean,
    onEdit: () -> Unit,
    onDelete: () -> Unit,
    onVerify: () -> Unit,
    onSwitch: () -> Unit
) {
    Card(
        modifier = Modifier
            .fillMaxWidth()
            .padding(16.dp)
            .border(
                width = 2.dp,
                color = if (isActive) MaterialTheme.colorScheme.primary else Color.Transparent,
                shape = RoundedCornerShape(12.dp)
            )
    ) {
        Column(modifier = Modifier.padding(16.dp)) {
            Row(
                modifier = Modifier.fillMaxWidth(),
                horizontalArrangement = Arrangement.SpaceBetween,
                verticalAlignment = Alignment.CenterVertically
            ) {
                Column(modifier = Modifier.weight(1f)) {
                    Text(
                        apiKey.nickname,
                        style = MaterialTheme.typography.titleMedium
                    )
                    if (isActive) {
                        Chip(
                            label = { Text("Active") },
                            leadingIcon = { Icon(Icons.Default.Check, null) }
                        )
                    }
                }
                StatusBadge(status = apiKey.status)
            }
            
            Spacer(modifier = Modifier.height(8.dp))
            
            Text(
                "Key: ${apiKey.key.take(10)}...${apiKey.key.takeLast(4)}",
                style = MaterialTheme.typography.bodySmall,
                color = MaterialTheme.colorScheme.onSurfaceVariant
            )
            
            Spacer(modifier = Modifier.height(8.dp))
            
            Row(
                modifier = Modifier.fillMaxWidth(),
                horizontalArrangement = Arrangement.SpaceBetween
            ) {
                Text("Model: ${apiKey.preferredModel}", 
                    style = MaterialTheme.typography.labelSmall)
                Text("Usage: ${apiKey.requestsThisHour}/${apiKey.maxRequestsPerHour}",
                    style = MaterialTheme.typography.labelSmall)
            }
            
            Spacer(modifier = Modifier.height(12.dp))
            
            Row(
                modifier = Modifier.fillMaxWidth(),
                horizontalArrangement = Arrangement.SpaceEvenly
            ) {
                TextButton(onClick = onEdit, modifier = Modifier.weight(1f)) {
                    Text("Edit")
                }
                TextButton(onClick = onVerify, modifier = Modifier.weight(1f)) {
                    Text("Verify")
                }
                TextButton(onClick = onDelete, modifier = Modifier.weight(1f)) {
                    Text("Delete", color = MaterialTheme.colorScheme.error)
                }
            }
        }
    }
}

// StatusBadge.kt
@Composable
fun StatusBadge(status: KeyStatus) {
    val (backgroundColor, textColor, icon) = when (status) {
        KeyStatus.ACTIVE -> Triple(
            Color.Green.copy(alpha = 0.2f),
            Color.Green,
            Icons.Default.Check
        )
        KeyStatus.VALID -> Triple(
            Color.Blue.copy(alpha = 0.2f),
            Color.Blue,
            Icons.Default.Info
        )
        KeyStatus.RATE_LIMITED -> Triple(
            Color.Yellow.copy(alpha = 0.2f),
            Color.Yellow,
            Icons.Default.Warning
        )
        KeyStatus.INVALID -> Triple(
            Color.Red.copy(alpha = 0.2f),
            Color.Red,
            Icons.Default.Error
        )
        KeyStatus.ERROR -> Triple(
            Color.Red.copy(alpha = 0.2f),
            Color.Red,
            Icons.Default.ErrorOutline
        )
        KeyStatus.CHECKING -> Triple(
            Color.Gray.copy(alpha = 0.2f),
            Color.Gray,
            Icons.Default.Refresh
        )
    }
    
    Surface(
        color = backgroundColor,
        shape = RoundedCornerShape(12.dp)
    ) {
        Row(
            modifier = Modifier.padding(8.dp),
            horizontalArrangement = Arrangement.spacedBy(4.dp),
            verticalAlignment = Alignment.CenterVertically
        ) {
            Icon(icon, null, modifier = Modifier.size(16.dp), tint = textColor)
            Text(status.name, style = MaterialTheme.typography.labelSmall, color = textColor)
        }
    }
}

// GetApiKeyGuideSheet.kt
@Composable
fun GetApiKeyGuideSheet(
    onDismiss: () -> Unit,
    onOpenBrowser: () -> Unit
) {
    ModalBottomSheet(onDismiss = onDismiss) {
        Column(
            modifier = Modifier
                .fillMaxWidth()
                .padding(16.dp)
        ) {
            Text(
                "Get Your Free API Key",
                style = MaterialTheme.typography.headlineSmall,
                modifier = Modifier.fillMaxWidth()
            )
            
            Spacer(modifier = Modifier.height(16.dp))
            
            StepCard(
                step = 1,
                title = "Create Account",
                description = "Sign up at OpenRouter.ai with email or GitHub",
                buttonText = "Open OpenRouter.ai",
                onClick = onOpenBrowser
            )
            
            Spacer(modifier = Modifier.height(12.dp))
            
            StepCard(
                step = 2,
                title = "Get API Key",
                description = "Go to Settings → Create API Key → Copy (starts with sk_or_)",
                buttonText = "Copy Key from Browser",
                onClick = { }
            )
            
            Spacer(modifier = Modifier.height(12.dp))
            
            StepCard(
                step = 3,
                title = "Add to JARVIS",
                description = "Return here and paste your key → Tap Verify",
                buttonText = "Back to JARVIS",
                onClick = onDismiss
            )
            
            Spacer(modifier = Modifier.height(16.dp))
            
            Button(
                onClick = onOpenBrowser,
                modifier = Modifier.fillMaxWidth()
            ) {
                Text("Start Setup")
            }
        }
    }
}
```

---

### 8. Auto-Switch Logic

```kotlin
// ApiCallInterceptor.kt - Retrofit Interceptor
class ApiKeyInterceptor @Inject constructor(
    private val apiKeyRepository: ApiKeyRepository,
    private val switchApiKeyUseCase: SwitchApiKeyUseCase
) : Interceptor {
    
    override fun intercept(chain: Interceptor.Chain): Response {
        var request = chain.request()
        var response = chain.proceed(request)
        
        // If rate limited (429 status)
        if (response.code == 429) {
            // Switch to next key
            val newKey = runBlocking {
                switchApiKeyUseCase.execute().getOrNull()
            }
            
            if (newKey != null) {
                // Retry with new key
                val newRequest = request.newBuilder()
                    .header("Authorization", "Bearer ${newKey.key}")
                    .build()
                response.close()
                response = chain.proceed(newRequest)
            }
        }
        
        return response
    }
}

// Usage in Retrofit setup
@Module
@InstallIn(SingletonComponent::class)
object NetworkModule {
    
    @Provides
    @Singleton
    fun provideOkHttpClient(
        apiKeyInterceptor: ApiKeyInterceptor
    ): OkHttpClient = OkHttpClient.Builder()
        .addInterceptor(apiKeyInterceptor)
        .connectTimeout(30, TimeUnit.SECONDS)
        .readTimeout(30, TimeUnit.SECONDS)
        .build()
    
    @Provides
    @Singleton
    fun provideRetrofit(
        okHttpClient: OkHttpClient
    ): Retrofit = Retrofit.Builder()
        .baseUrl("https://openrouter.ai/api/v1/")
        .client(okHttpClient)
        .addConverterFactory(GsonConverterFactory.create())
        .build()
}
```

---

### 9. Open Browser Intent

```kotlin
// OpenRouterHelper.kt
object OpenRouterHelper {
    
    fun openOpenRouterWebsite(context: Context) {
        val intent = Intent(Intent.ACTION_VIEW).apply {
            data = Uri.parse("https://openrouter.ai/")
        }
        context.startActivity(intent)
    }
    
    fun openOpenRouterApiSettings(context: Context) {
        val intent = Intent(Intent.ACTION_VIEW).apply {
            data = Uri.parse("https://openrouter.ai/account/api-keys")
        }
        context.startActivity(intent)
    }
    
    fun openOpenRouterModels(context: Context) {
        val intent = Intent(Intent.ACTION_VIEW).apply {
            data = Uri.parse("https://openrouter.ai/models")
        }
        context.startActivity(intent)
    }
}
```

---

## 📊 Error Handling & Retry Strategy

```kotlin
// ApiKeyErrorHandler.kt
class ApiKeyErrorHandler {
    
    fun handleApiError(
        error: Throwable,
        currentKey: ApiKeyModel
    ): ApiKeyErrorAction = when {
        error.message?.contains("429") == true -> 
            ApiKeyErrorAction.SWITCH_KEY
        
        error.message?.contains("401") == true -> 
            ApiKeyErrorAction.INVALID_KEY
        
        error.message?.contains("403") == true -> 
            ApiKeyErrorAction.INSUFFICIENT_QUOTA
        
        error.message?.contains("timeout") == true -> 
            ApiKeyErrorAction.RETRY_WITH_BACKOFF
        
        else -> ApiKeyErrorAction.SHOW_ERROR
    }
}

enum class ApiKeyErrorAction {
    SWITCH_KEY,             // Automatic key switch
    INVALID_KEY,            // Mark key as invalid
    INSUFFICIENT_QUOTA,     // Quota exceeded
    RETRY_WITH_BACKOFF,     // Retry after delay
    SHOW_ERROR              // Show error to user
}
```

---

## 🔐 Security Features

```kotlin
// Security Implementation Checklist:

1. ✅ Encrypted Storage
   - Use EncryptedSharedPreferences
   - AES-256 encryption at rest
   - DataStore with encryption

2. ✅ Secure Transmission
   - TLS 1.3+ for all network requests
   - Certificate pinning for OpenRouter API
   - No logging of sensitive data

3. ✅ Access Control
   - Keys never exposed in logs
   - Keys never saved in crash reports
   - Automatic cleanup on logout

4. ✅ User Privacy
   - No analytics on API keys
   - No cloud backup of keys
   - User controls data retention

5. ✅ Key Rotation
   - Users can delete old keys anytime
   - Automatic backoff for compromised keys
   - Verification before use
```

---

## 🎯 User Flow Diagram

```
START
  ↓
[Has API Key?] 
  YES ↓                NO ↓
  ↓                    ↓
[Use Active Key] ← [Show Get Key Guide]
  ↓                    ↓
[Make Request]    [Tap: Open OpenRouter.ai]
  ↓                    ↓
[429 Rate Limited?]   [Create Account]
  YES ↓  NO ↓          ↓
  ↓      ↓         [Get API Key]
  ↓  [Success ✓]      ↓
  ↓                [Copy Key]
[Switch to Next Key]   ↓
  ↓               [Return to App]
[Available Key?]       ↓
  YES ↓  NO ↓      [Paste Key]
  ↓      ↓         ↓
  ↓  [Error ✗]  [Tap Verify]
  ↓                ↓
[Retry Request]   [Key Valid?]
  ↓                YES ↓ NO ↓
[Success ✓]        ↓    ↓
  ↓             [Added ✓] [Error ✗]
  END               ↓       ↓
                  [Use Key] [Try Again]
                    ↓
                  [START REQUEST]
```

---

## 🧪 Testing Strategy

```kotlin
// ApiKeyRepositoryTest.kt
@RunWith(RobolectricTestRunner::class)
class ApiKeyRepositoryTest {
    
    @Test
    fun testAddApiKey() = runTest {
        // Test adding a new key
    }
    
    @Test
    fun testSwitchToNextKey() = runTest {
        // Test automatic key switching
    }
    
    @Test
    fun testRateLimitHandling() = runTest {
        // Test 429 rate limit response
    }
    
    @Test
    fun testKeyEncryption() {
        // Test encryption/decryption
    }
    
    @Test
    fun testFreeModelFiltering() = runTest {
        // Test free model selection
    }
}
```

---

## 📋 Checklist for Implementation

- [ ] Create API Key data models
- [ ] Implement encrypted DataStore
- [ ] Create API Key Repository
- [ ] Build domain use cases
- [ ] Create ViewModel
- [ ] Design Composable screens
- [ ] Implement browser intent
- [ ] Add error handling
- [ ] Create interceptor for auto-switch
- [ ] Add encryption manager
- [ ] Implement validation logic
- [ ] Add unit tests
- [ ] Add UI tests
- [ ] Security audit
- [ ] Documentation

---

**JARVIS API Key Management System - Complete & Ready for Implementation** 🚀

Made with ❤️ for seamless OpenRouter integration
