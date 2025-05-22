import React, { useState } from 'react';
import { 
  BarChart, Bar, XAxis, YAxis, CartesianGrid, Tooltip, ResponsiveContainer,
  LineChart, Line, PieChart, Pie, Cell, Area, AreaChart
} from 'recharts';
import { 
  Database, Brain, Shield, Users, Settings, CreditCard, 
  Download, Share2, Play, Pause, Plus, Search, Filter,
  TrendingUp, DollarSign, Globe, Smartphone, Zap, Lock, Languages, MessageCircle
} from 'lucide-react';

// Ajout assistant IA (ChatGPT) : état et logique
const MiaKapitalPlatform = () => {
  const [activeTab, setActiveTab] = useState('dashboard');
  const [selectedWorkflow, setSelectedWorkflow] = useState(null);
  const [paymentMethod, setPaymentMethod] = useState('');
  const [isProcessing, setIsProcessing] = useState(false);
  const [currentLanguage, setCurrentLanguage] = useState('en');
  const [isTranslating, setIsTranslating] = useState(false);

  // Chat assistant state
  const [chatOpen, setChatOpen] = useState(false);
  const [chatMessages, setChatMessages] = useState([
    { role: 'assistant', content: "👋 Hello! I'm your AI assistant. How can I help you on MIA KAPITAL today?" }
  ]);
  const [chatInput, setChatInput] = useState('');

  // Translation object with multiple languages
  const translations = {
    en: {
      dashboard: 'Dashboard',
      workflows: 'Analytics Studio',
      dataSources: 'Data Sources',
      aiModels: 'AI Models',
      marketplace: 'Marketplace',
      payments: 'Payments',
      security: 'Security',
      users: 'Users',
      settings: 'Settings',
      analyticsDashboard: 'Analytics Dashboard',
      exportToBlog: 'Export to Blog',
      share: 'Share',
      totalRevenue: 'Total Revenue',
      activeUsers: 'Active Users',
      activeWorkflows: 'Active Workflows',
      revenueAnalytics: 'Revenue Analytics',
      departmentUsage: 'Department Usage',
      newWorkflow: 'New Workflow',
      workflowDesigner: 'Workflow Designer',
      dragDropMessage: 'Drag and drop components to build your analytics workflow',
      dataSource: 'Data Source',
      transform: 'Transform',
      aiModel: 'AI Model',
      paymentMethods: 'Payment Methods',
      paymentSecurity: 'Payment Security',
      encrypted: 'Encrypted',
      pciCompliant: 'PCI Compliant',
      mobileReady: 'Mobile Ready',
      selectMethod: 'Select Method',
      processing: 'Processing...',
      comingSoon: 'Coming Soon',
      underDevelopment: 'This section is under development',
      finance: 'Finance',
      marketing: 'Marketing',
      operations: 'Operations',
      hr: 'HR',
      autoTranslate: 'Auto Translate',
      language: 'Language',
      connected: 'Connected',
      pending: 'Pending',
      running: 'Running',
      completed: 'Completed'
    },
    fr: {
      dashboard: 'Tableau de Bord',
      workflows: 'Studio Analytique',
      dataSources: 'Sources de Données',
      aiModels: 'Modèles IA',
      marketplace: 'Place de Marché',
      payments: 'Paiements',
      security: 'Sécurité',
      users: 'Utilisateurs',
      settings: 'Paramètres',
      analyticsDashboard: 'Tableau de Bord Analytique',
      exportToBlog: 'Exporter vers Blog',
      share: 'Partager',
      totalRevenue: 'Chiffre d\'Affaires Total',
      activeUsers: 'Utilisateurs Actifs',
      activeWorkflows: 'Workflows Actifs',
      revenueAnalytics: 'Analyse des Revenus',
      departmentUsage: 'Usage par Département',
      newWorkflow: 'Nouveau Workflow',
      workflowDesigner: 'Concepteur de Workflow',
      dragDropMessage: 'Glissez-déposez les composants pour créer votre workflow analytique',
      dataSource: 'Source de Données',
      transform: 'Transformer',
      aiModel: 'Modèle IA',
      paymentMethods: 'Méthodes de Paiement',
      paymentSecurity: 'Sécurité des Paiements',
      encrypted: 'Chiffré',
      pciCompliant: 'Conforme PCI',
      mobileReady: 'Mobile Ready',
      selectMethod: 'Sélectionner Méthode',
      processing: 'Traitement...',
      comingSoon: 'Bientôt Disponible',
      underDevelopment: 'Cette section est en cours de développement',
      finance: 'Finance',
      marketing: 'Marketing',
      operations: 'Opérations',
      hr: 'RH',
      autoTranslate: 'Traduction Auto',
      language: 'Langue',
      connected: 'Connecté',
      pending: 'En attente',
      running: 'En cours',
      completed: 'Terminé'
    },
    es: {
      dashboard: 'Panel de Control',
      workflows: 'Estudio de Análisis',
      dataSources: 'Fuentes de Datos',
      aiModels: 'Modelos de IA',
      marketplace: 'Mercado',
      payments: 'Pagos',
      security: 'Seguridad',
      users: 'Usuarios',
      settings: 'Configuración',
      analyticsDashboard: 'Panel de Análisis',
      exportToBlog: 'Exportar a Blog',
      share: 'Compartir',
      totalRevenue: 'Ingresos Totales',
      activeUsers: 'Usuarios Activos',
      activeWorkflows: 'Flujos de Trabajo Activos',
      revenueAnalytics: 'Análisis de Ingresos',
      departmentUsage: 'Uso por Departamento',
      newWorkflow: 'Nuevo Flujo de Trabajo',
      workflowDesigner: 'Diseñador de Flujo de Trabajo',
      dragDropMessage: 'Arrastra y suelta componentes para crear tu flujo de trabajo analítico',
      dataSource: 'Fuente de Datos',
      transform: 'Transformar',
      aiModel: 'Modelo IA',
      paymentMethods: 'Métodos de Pago',
      paymentSecurity: 'Seguridad de Pagos',
      encrypted: 'Encriptado',
      pciCompliant: 'Cumple PCI',
      mobileReady: 'Listo para Móvil',
      selectMethod: 'Seleccionar Método',
      processing: 'Procesando...',
      comingSoon: 'Próximamente',
      underDevelopment: 'Esta sección está en desarrollo',
      finance: 'Finanzas',
      marketing: 'Marketing',
      operations: 'Operaciones',
      hr: 'RRHH',
      autoTranslate: 'Traducir Auto',
      language: 'Idioma',
      connected: 'Conectado',
      pending: 'Pendiente',
      running: 'Ejecutando',
      completed: 'Completado'
    },
    ar: {
      dashboard: 'لوحة التحكم',
      workflows: 'استوديو التحليلات',
      dataSources: 'مصادر البيانات',
      aiModels: 'نماذج الذكاء الاصطناعي',
      marketplace: 'السوق',
      payments: 'المدفوعات',
      security: 'الأمان',
      users: 'المستخدمون',
      settings: 'الإعدادات',
      analyticsDashboard: 'لوحة تحكم التحليلات',
      exportToBlog: 'تصدير للمدونة',
      share: 'مشاركة',
      totalRevenue: 'إجمالي الإيرادات',
      activeUsers: 'المستخدمون النشطون',
      activeWorkflows: 'سير العمل النشط',
      revenueAnalytics: 'تحليل الإيرادات',
      departmentUsage: 'الاستخدام حسب القسم',
      newWorkflow: 'سير عمل جديد',
      workflowDesigner: 'مصمم سير العمل',
      dragDropMessage: 'اسحب وأفلت المكونات لبناء سير العمل التحليلي الخاص بك',
      dataSource: 'مصدر البيانات',
      transform: 'تحويل',
      aiModel: 'نموذج الذكاء الاصطناعي',
      paymentMethods: 'طرق الدفع',
      paymentSecurity: 'أمان المدفوعات',
      encrypted: 'مشفر',
      pciCompliant: 'متوافق مع PCI',
      mobileReady: 'جاهز للهاتف المحمول',
      selectMethod: 'اختر الطريقة',
      processing: 'جاري المعالجة...',
      comingSoon: 'قريباً',
      underDevelopment: 'هذا القسم قيد التطوير',
      finance: 'المالية',
      marketing: 'التسويق',
      operations: 'العمليات',
      hr: 'الموارد البشرية',
      autoTranslate: 'ترجمة تلقائية',
      language: 'اللغة',
      connected: 'متصل',
      pending: 'في الانتظار',
      running: 'يعمل',
      completed: 'مكتمل'
    },
    pt: {
      dashboard: 'Painel',
      workflows: 'Estúdio de Análise',
      dataSources: 'Fontes de Dados',
      aiModels: 'Modelos de IA',
      marketplace: 'Mercado',
      payments: 'Pagamentos',
      security: 'Segurança',
      users: 'Usuários',
      settings: 'Configurações',
      analyticsDashboard: 'Painel de Análise',
      exportToBlog: 'Exportar para Blog',
      share: 'Compartilhar',
      totalRevenue: 'Receita Total',
      activeUsers: 'Usuários Ativos',
      activeWorkflows: 'Fluxos de Trabalho Ativos',
      revenueAnalytics: 'Análise de Receita',
      departmentUsage: 'Uso por Departamento',
      newWorkflow: 'Novo Fluxo de Trabalho',
      workflowDesigner: 'Designer de Fluxo de Trabalho',
      dragDropMessage: 'Arraste e solte componentes para construir seu fluxo de trabalho analítico',
      dataSource: 'Fonte de Dados',
      transform: 'Transformar',
      aiModel: 'Modelo IA',
      paymentMethods: 'Métodos de Pagamento',
      paymentSecurity: 'Segurança de Pagamentos',
      encrypted: 'Criptografado',
      pciCompliant: 'Conforme PCI',
      mobileReady: 'Pronto para Mobile',
      selectMethod: 'Selecionar Método',
      processing: 'Processando...',
      comingSoon: 'Em Breve',
      underDevelopment: 'Esta seção está em desenvolvimento',
      finance: 'Finanças',
      marketing: 'Marketing',
      operations: 'Operações',
      hr: 'RH',
      autoTranslate: 'Tradução Auto',
      language: 'Idioma',
      connected: 'Conectado',
      pending: 'Pendente',
      running: 'Executando',
      completed: 'Concluído'
    }
  };

  const languages = [
    { code: 'en', name: 'English', flag: '🇺🇸' },
    { code: 'fr', name: 'Français', flag: '🇫🇷' },
    { code: 'es', name: 'Español', flag: '🇪🇸' },
    { code: 'ar', name: 'العربية', flag: '🇸🇦' },
    { code: 'pt', name: 'Português', flag: '🇵🇹' }
  ];

  const t = (key) => translations[currentLanguage][key] || key;

  const handleLanguageChange = async (langCode) => {
    setIsTranslating(true);
    // Simulate API call for translation
    await new Promise(resolve => setTimeout(resolve, 800));
    setCurrentLanguage(langCode);
    setIsTranslating(false);
  };

  const autoTranslate = async () => {
    setIsTranslating(true);
    // Simulate auto-translation detection and processing
    await new Promise(resolve => setTimeout(resolve, 1500));
    
    // Simple language detection simulation (you would use a real API)
    const detectedLang = ['fr', 'es', 'ar', 'pt'][Math.floor(Math.random() * 4)];
    setCurrentLanguage(detectedLang);
    setIsTranslating(false);
    
    alert(`Auto-detected and translated to ${languages.find(l => l.code === detectedLang)?.name}`);
  };

  // Sample data for analytics
  const analyticsData = [
    { name: 'Jan', revenue: 4000, users: 240, conversion: 2.4 },
    { name: 'Feb', revenue: 3000, users: 139, conversion: 4.3 },
    { name: 'Mar', revenue: 2000, users: 980, conversion: 2.9 },
    { name: 'Apr', revenue: 2780, users: 390, conversion: 3.8 },
    { name: 'May', revenue: 1890, users: 480, conversion: 4.8 },
    { name: 'Jun', revenue: 2390, users: 380, conversion: 3.8 }
  ];

  const pieData = [
    { name: 'Finance', value: 35, color: '#8884d8' },
    { name: 'Marketing', value: 25, color: '#82ca9d' },
    { name: 'Operations', value: 20, color: '#ffc658' },
    { name: 'HR', value: 20, color: '#ff7300' }
  ];

  const workflows = [
    { id: 1, name: 'Customer Segmentation', status: 'running', department: 'Marketing' },
    { id: 2, name: 'Revenue Forecast', status: 'completed', department: 'Finance' },
    { id: 3, name: 'Churn Prediction', status: 'pending', department: 'Operations' },
    { id: 4, name: 'Performance Analysis', status: 'running', department: 'HR' }
  ];

  const paymentMethods = [
    { id: 'momo', name: 'MTN MoMo Pay', icon: '📱', color: 'bg-yellow-500' },
    { id: 'moov', name: 'Moov Money', icon: '💳', color: 'bg-blue-500' },
    { id: 'orange', name: 'Orange Money', icon: '🟠', color: 'bg-orange-500' },
    { id: 'visa', name: 'Visa Card', icon: '💳', color: 'bg-purple-500' },
    { id: 'mastercard', name: 'Mastercard', icon: '💳', color: 'bg-red-500' }
  ];

  const exportToBlog = () => {
    const blogContent = {
      title: "MIA KAPITAL Analytics Platform - Dashboard Export",
      date: new Date().toISOString(),
      language: currentLanguage,
      analytics: analyticsData,
      workflows: workflows,
      departments: pieData,
      metadata: {
        activeWorkflows: workflows.filter(w => w.status === 'running').length,
        completedAnalytics: workflows.filter(w => w.status === 'completed').length,
        totalRevenue: analyticsData.reduce((sum, item) => sum + item.revenue, 0)
      }
    };
    
    const blob = new Blob([JSON.stringify(blogContent, null, 2)], { type: 'application/json' });
    const url = URL.createObjectURL(blob);
    const a = document.createElement('a');
    a.href = url;
    a.download = `mia-kapital-blog-export-${currentLanguage}.json`;
    a.click();
  };

  const processPayment = async (method) => {
    setIsProcessing(true);
    setPaymentMethod(method);
    
    // Simulate payment processing
    setTimeout(() => {
      setIsProcessing(false);
      alert(`${t('processing')} ${paymentMethods.find(p => p.id === method)?.name}!`);
    }, 2000);
  };

  const Sidebar = () => (
    <div className={`w-64 bg-gray-900 text-white p-6 min-h-screen ${currentLanguage === 'ar' ? 'text-right' : ''}`}>
      <div className="flex items-center mb-6">
        <Brain className="w-8 h-8 text-blue-400 mr-3" />
        <h1 className="text-xl font-bold">MIA KAPITAL</h1>
      </div>
      
      {/* Language Selector */}
      <div className="mb-6 border-b border-gray-700 pb-4">
        <div className="flex items-center justify-between mb-3">
          <span className="text-sm text-gray-300 flex items-center">
            <Languages className="w-4 h-4 mr-2" />
            {t('language')}
          </span>
          <button
            onClick={autoTranslate}
            disabled={isTranslating}
            className="text-xs bg-blue-600 hover:bg-blue-700 px-2 py-1 rounded flex items-center"
          >
            {isTranslating ? (
              <div className="animate-spin w-3 h-3 border border-white border-t-transparent rounded-full mr-1"></div>
            ) : (
              <Globe className="w-3 h-3 mr-1" />
            )}
            {t('autoTranslate')}
          </button>
        </div>
        <select
          value={currentLanguage}
          onChange={(e) => handleLanguageChange(e.target.value)}
          className="w-full bg-gray-800 text-white p-2 rounded text-sm border border-gray-600"
          disabled={isTranslating}
        >
          {languages.map(lang => (
            <option key={lang.code} value={lang.code}>
              {lang.flag} {lang.name}
            </option>
          ))}
        </select>
        {isTranslating && (
          <div className="text-xs text-blue-400 mt-2 flex items-center">
            <div className="animate-spin w-3 h-3 border border-blue-400 border-t-transparent rounded-full mr-2"></div>
            Translating...
          </div>
        )}
      </div>
      
      <nav className="space-y-2">
        {[
          { id: 'dashboard', label: t('dashboard'), icon: BarChart },
          { id: 'workflows', label: t('workflows'), icon: Zap },
          { id: 'data', label: t('dataSources'), icon: Database },
          { id: 'ai', label: t('aiModels'), icon: Brain },
          { id: 'marketplace', label: t('marketplace'), icon: Globe },
          { id: 'payments', label: t('payments'), icon: CreditCard },
          { id: 'security', label: t('security'), icon: Shield },
          { id: 'users', label: t('users'), icon: Users },
          { id: 'settings', label: t('settings'), icon: Settings }
        ].map(({ id, label, icon: Icon }) => (
          <button
            key={id}
            onClick={() => setActiveTab(id)}
            className={`w-full flex items-center p-3 rounded-lg transition-colors ${
              activeTab === id ? 'bg-blue-600' : 'hover:bg-gray-800'
            } ${currentLanguage === 'ar' ? 'flex-row-reverse' : ''}`}
          >
            <Icon className={`w-5 h-5 ${currentLanguage === 'ar' ? 'ml-3' : 'mr-3'}`} />
            {label}
          </button>
        ))}
      </nav>
      {/* Bouton assistant IA */}
      <button
        className="fixed bottom-8 left-8 bg-white text-blue-600 rounded-full shadow-lg p-3 hover:bg-blue-100 z-50 flex items-center"
        style={{ minWidth: 48 }}
        onClick={() => setChatOpen(true)}
        aria-label="Open AI Assistant"
      >
        <MessageCircle className="w-6 h-6" />
      </button>
    </div>
  );

  const Dashboard = () => (
    <div className={`space-y-6 ${currentLanguage === 'ar' ? 'text-right' : ''}`}>
      <div className="flex justify-between items-center">
        <h2 className="text-3xl font-bold text-gray-800">{t('analyticsDashboard')}</h2>
        <div className="flex space-x-3">
          <button 
            onClick={exportToBlog}
            className="flex items-center px-4 py-2 bg-green-600 text-white rounded-lg hover:bg-green-700"
          >
            <Download className={`w-4 h-4 ${currentLanguage === 'ar' ? 'ml-2' : 'mr-2'}`} />
            {t('exportToBlog')}
          </button>
          <button className="flex items-center px-4 py-2 bg-blue-600 text-white rounded-lg hover:bg-blue-700">
            <Share2 className={`w-4 h-4 ${currentLanguage === 'ar' ? 'ml-2' : 'mr-2'}`} />
            {t('share')}
          </button>
        </div>
      </div>

      <div className="grid grid-cols-1 md:grid-cols-4 gap-6">
        <div className="bg-white p-6 rounded-xl shadow-sm border">
          <div className="flex items-center justify-between">
            <div>
              <p className="text-gray-600">{t('totalRevenue')}</p>
              <p className="text-2xl font-bold text-gray-800">$14,060</p>
            </div>
            <TrendingUp className="w-8 h-8 text-green-500" />
          </div>
        </div>
        
        <div className="bg-white p-6 rounded-xl shadow-sm border">
          <div className="flex items-center justify-between">
            <div>
              <p className="text-gray-600">{t('activeUsers')}</p>
              <p className="text-2xl font-bold text-gray-800">2,409</p>
            </div>
            <Users className="w-8 h-8 text-blue-500" />
          </div>
        </div>
        
        <div className="bg-white p-6 rounded-xl shadow-sm border">
          <div>
            <p className="text-gray-600">{t('activeWorkflows')}</p>
            <p className="text-2xl font-bold text-gray-800">{workflows.filter(w => w.status === 'running').length}</p>
          </div>
        </div>
        
        <div className="bg-white p-6 rounded-xl shadow-sm border">
          <div>
            <p className="text-gray-600">{t('aiModels')}</p>
            <p className="text-2xl font-bold text-gray-800">12</p>
          </div>
        </div>
      </div>

      <div className="grid grid-cols-1 lg:grid-cols-2 gap-6">
        <div className="bg-white p-6 rounded-xl shadow-sm border">
          <h3 className="text-lg font-semibold mb-4">{t('revenueAnalytics')}</h3>
          <ResponsiveContainer width="100%" height={300}>
            <AreaChart data={analyticsData}>
              <CartesianGrid strokeDasharray="3 3" />
              <XAxis dataKey="name" />
              <YAxis />
              <Tooltip />
              <Area type="monotone" dataKey="revenue" stroke="#8884d8" fill="#8884d8" fillOpacity={0.3} />
            </AreaChart>
          </ResponsiveContainer>
        </div>

        <div className="bg-white p-6 rounded-xl shadow-sm border">
          <h3 className="text-lg font-semibold mb-4">{t('departmentUsage')}</h3>
          <ResponsiveContainer width="100%" height={300}>
            <PieChart>
              <Pie
                data={pieData.map(item => ({
                  ...item,
                  name: t(item.name.toLowerCase())
                }))}
                cx="50%"
                cy="50%"
                outerRadius={80}
                dataKey="value"
              >
                {pieData.map((entry, index) => (
                  <Cell key={`cell-${index}`} fill={entry.color} />
                ))}
              </Pie>
              <Tooltip />
            </PieChart>
          </ResponsiveContainer>
        </div>
      </div>
    </div>
  );

  const WorkflowStudio = () => (
    <div className={`space-y-6 ${currentLanguage === 'ar' ? 'text-right' : ''}`}>
      <div className="flex justify-between items-center">
        <h2 className="text-3xl font-bold text-gray-800">{t('workflows')}</h2>
        <button className="flex items-center px-4 py-2 bg-blue-600 text-white rounded-lg hover:bg-blue-700">
          <Plus className={`w-4 h-4 ${currentLanguage === 'ar' ? 'ml-2' : 'mr-2'}`} />
          {t('newWorkflow')}
        </button>
      </div>

      <div className="grid grid-cols-1 lg:grid-cols-3 gap-6">
        <div className="lg:col-span-2 bg-white p-6 rounded-xl shadow-sm border">
          <h3 className="text-lg font-semibold mb-4">{t('workflowDesigner')}</h3>
          <div className="border-2 border-dashed border-gray-300 rounded-lg p-8 text-center">
            <Brain className="w-16 h-16 text-gray-400 mx-auto mb-4" />
            <p className="text-gray-600 mb-4">{t('dragDropMessage')}</p>
            <div className="flex justify-center space-x-4">
              <div className="bg-blue-100 p-3 rounded-lg cursor-pointer hover:bg-blue-200">
                <Database className="w-6 h-6 text-blue-600" />
                <span className="text-xs text-blue-600">{t('dataSource')}</span>
              </div>
              <div className="bg-green-100 p-3 rounded-lg cursor-pointer hover:bg-green-200">
                <Filter className="w-6 h-6 text-green-600" />
                <span className="text-xs text-green-600">{t('transform')}</span>
              </div>
              <div className="bg-purple-100 p-3 rounded-lg cursor-pointer hover:bg-purple-200">
                <Brain className="w-6 h-6 text-purple-600" />
                <span className="text-xs text-purple-600">{t('aiModel')}</span>
              </div>
            </div>
          </div>
        </div>

        <div className="bg-white p-6 rounded-xl shadow-sm border">
          <h3 className="text-lg font-semibold mb-4">{t('activeWorkflows')}</h3>
          <div className="space-y-3">
            {workflows.map((workflow) => (
              <div key={workflow.id} className="border rounded-lg p-3">
                <div className="flex justify-between items-center">
                  <div>
                    <p className="font-semibold text-sm">{workflow.name}</p>
                    <p className="text-xs text-gray-600">{t(workflow.department.toLowerCase())}</p>
                  </div>
                  <div className="flex items-center space-x-2">
                    <span className={`w-2 h-2 rounded-full ${
                      workflow.status === 'running' ? 'bg-green-500' :
                      workflow.status === 'completed' ? 'bg-blue-500' : 'bg-yellow-500'
                    }`}></span>
                    <span className="text-xs text-gray-500">{t(workflow.status)}</span>
                    {workflow.status === 'running' ? (
                      <Pause className="w-4 h-4 text-gray-600 cursor-pointer" />
                    ) : (
                      <Play className="w-4 h-4 text-gray-600 cursor-pointer" />
                    )}
                  </div>
                </div>
              </div>
            ))}
          </div>
        </div>
      </div>
    </div>
  );

  const PaymentSection = () => (
    <div className={`space-y-6 ${currentLanguage === 'ar' ? 'text-right' : ''}`}>
      <h2 className="text-3xl font-bold text-gray-800">{t('paymentMethods')}</h2>
      
      <div className="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-6">
        {paymentMethods.map((method) => (
          <div key={method.id} className="bg-white p-6 rounded-xl shadow-sm border hover:shadow-md transition-shadow">
            <div className={`w-12 h-12 ${method.color} rounded-lg flex items-center justify-center text-white text-xl mb-4`}>
              {method.icon}
            </div>
            <h3 className="text-lg font-semibold mb-2">{method.name}</h3>
            <p className="text-gray-600 text-sm mb-4">
              {method.id === 'momo' && 'Mobile money payments via MTN network'}
              {method.id === 'moov' && 'Secure mobile payments with Moov'}
              {method.id === 'orange' && 'Orange Money mobile wallet'}
              {method.id === 'visa' && 'International Visa card payments'}
              {method.id === 'mastercard' && 'Worldwide Mastercard acceptance'}
            </p>
            <button 
              onClick={() => processPayment(method.id)}
              disabled={isProcessing && paymentMethod === method.id}
              className={`w-full py-2 px-4 rounded-lg font-semibold transition-colors ${
                isProcessing && paymentMethod === method.id
                  ? 'bg-gray-400 text-gray-600 cursor-not-allowed'
                  : 'bg-blue-600 text-white hover:bg-blue-700'
              }`}
            >
              {isProcessing && paymentMethod === method.id ? (
                <div className="flex items-center justify-center">
                  <div className="animate-spin w-4 h-4 border-2 border-white border-t-transparent rounded-full mr-2"></div>
                  {t('processing')}
                </div>
              ) : (
                t('selectMethod')
              )}
            </button>
          </div>
        ))}
      </div>

      <div className="bg-white p-6 rounded-xl shadow-sm border">
        <h3 className="text-lg font-semibold mb-4">{t('paymentSecurity')}</h3>
        <div className="grid grid-cols-1 md:grid-cols-3 gap-4">
          <div className="flex items-center p-4 bg-green-50 rounded-lg">
            <Lock className="w-6 h-6 text-green-600 mr-3" />
            <div>
              <p className="font-semibold text-green-800">{t('encrypted')}</p>
              <p className="text-sm text-green-600">256-bit SSL encryption</p>
            </div>
          </div>
          <div className="flex items-center p-4 bg-blue-50 rounded-lg">
            <Shield className="w-6 h-6 text-blue-600 mr-3" />
            <div>
              <p className="font-semibold text-blue-800">{t('pciCompliant')}</p>
              <p className="text-sm text-blue-600">Industry standard security</p>
            </div>
          </div>
          <div className="flex items-center p-4 bg-purple-50 rounded-lg">
            <Smartphone className="w-6 h-6 text-purple-600 mr-3" />
            <div>
              <p className="font-semibold text-purple-800">{t('mobileReady')}</p>
              <p className="text-sm text-purple-600">Optimized for mobile payments</p>
            </div>
          </div>
        </div>
      </div>
    </div>
  );

  const DataSources = () => (
    <div className={`space-y-6 ${currentLanguage === 'ar' ? 'text-right' : ''}`}>
      <h2 className="text-3xl font-bold text-gray-800">{t('dataSources')}</h2>
      <div className="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-6">
        {[
          { name: 'PostgreSQL', type: 'Database', status: 'connected', icon: '🐘' },
          { name: 'Salesforce API', type: 'CRM', status: 'connected', icon: '☁️' },
          { name: 'SAP ERP', type: 'ERP', status: 'pending', icon: '🏢' },
          { name: 'Google Analytics', type: 'Web Analytics', status: 'connected', icon: '📊' },
          { name: 'Excel Files', type: 'Files', status: 'connected', icon: '📄' },
          { name: 'REST API', type: 'API', status: 'connected', icon: '🔗' }
        ].map((source, index) => (
          <div key={index} className="bg-white p-6 rounded-xl shadow-sm border">
            <div className="flex items-center justify-between mb-4">
              <div className="text-2xl">{source.icon}</div>
              <span className={`px-2 py-1 rounded-full text-xs ${
                source.status === 'connected' ? 'bg-green-100 text-green-800' : 'bg-yellow-100 text-yellow-800'
              }`}>
                {t(source.status)}
              </span>
            </div>
            <h3 className="font-semibold">{source.name}</h3>
            <p className="text-gray-600 text-sm">{source.type}</p>
          </div>
        ))}
      </div>
    </div>
  );

  // ChatGPT Assistant Logic (mock, à remplacer par appel API OpenAI si besoin)
  const handleChatSend = async () => {
    if (!chatInput.trim()) return;
    setChatMessages([...chatMessages, { role: 'user', content: chatInput }]);
    setChatInput('');
    // Simule une réponse IA
    setTimeout(() => {
      setChatMessages(msgs => [
        ...msgs,
        { role: 'assistant', content: "🤖 (AI) I'm here to assist you! For advanced analytics, try the 'Analytics Studio' tab or ask me anything about your data." }
      ]);
    }, 1200);
  };

  const ChatAssistant = () => (
    <div className="fixed bottom-8 left-24 z-50 w-80 bg-white rounded-xl shadow-2xl border border-blue-200 flex flex-col">
      <div className="flex items-center justify-between px-4 py-2 border-b">
        <div className="flex items-center gap-2">
          <Brain className="w-5 h-5 text-blue-500" />
          <span className="font-semibold text-blue-700">AI Assistant</span>
        </div>
        <button onClick={() => setChatOpen(false)} className="text-gray-400 hover:text-red-400 text-xl">&times;</button>
      </div>
      <div className="flex-1 overflow-y-auto px-4 py-2 space-y-2" style={{ maxHeight: 320 }}>
        {chatMessages.map((msg, idx) => (
          <div
            key={idx}
            className={`rounded-lg px-3 py-2 text-sm ${msg.role === 'assistant' ? 'bg-blue-50 text-blue-900 self-start' : 'bg-gray-100 text-gray-800 self-end'}`}
            style={{ alignSelf: msg.role === 'assistant' ? 'flex-start' : 'flex-end', maxWidth: '90%' }}
          >
            {msg.content}
          </div>
        ))}
      </div>
      <form
        className="flex border-t px-2 py-2 gap-2"
        onSubmit={e => { e.preventDefault(); handleChatSend(); }}
      >
        <input
          className="flex-1 border rounded-lg px-2 py-1 text-sm focus:outline-none"
          placeholder="Ask me anything..."
          value={chatInput}
          onChange={e => setChatInput(e.target.value)}
        />
        <button
          type="submit"
          className="bg-blue-600 text-white px-3 py-1 rounded-lg hover:bg-blue-700"
        >
          Send
        </button>
      </form>
    </div>
  );

  const renderContent = () => {
    switch(activeTab) {
      case 'dashboard': return <Dashboard />;
      case 'workflows': return <WorkflowStudio />;
      case 'data': return <DataSources />;
      case 'payments': return <PaymentSection />;
      case 'ai': return (
        <div className={`text-center py-12 ${currentLanguage === 'ar' ? 'text-right' : ''}`}>
          <Brain className="w-16 h-16 text-gray-400 mx-auto mb-4" />
          <h3 className="text-xl font-semibold text-gray-600">{t('aiModels')}</h3>
          <p className="text-gray-500">{t('underDevelopment')}</p>
        </div>
      );
      case 'marketplace': return (
        <div className={`text-center py-12 ${currentLanguage === 'ar' ? 'text-right' : ''}`}>
          <Globe className="w-16 h-16 text-gray-400 mx-auto mb-4" />
          <h3 className="text-xl font-semibold text-gray-600">{t('marketplace')}</h3>
          <p className="text-gray-500">{t('underDevelopment')}</p>
        </div>
      );
      case 'security': return (
        <div className={`text-center py-12 ${currentLanguage === 'ar' ? 'text-right' : ''}`}>
          <Shield className="w-16 h-16 text-gray-400 mx-auto mb-4" />
          <h3 className="text-xl font-semibold text-gray-600">{t('security')}</h3>
          <p className="text-gray-500">{t('underDevelopment')}</p>
        </div>
      );
      case 'users': return (
        <div className={`text-center py-12 ${currentLanguage === 'ar' ? 'text-right' : ''}`}>
          <Users className="w-16 h-16 text-gray-400 mx-auto mb-4" />
          <h3 className="text-xl font-semibold text-gray-600">{t('users')}</h3>
          <p className="text-gray-500">{t('underDevelopment')}</p>
        </div>
      );
      default: return (
        <div className={`text-center py-12 ${currentLanguage === 'ar' ? 'text-right' : ''}`}>
          <Settings className="w-16 h-16 text-gray-400 mx-auto mb-4" />
          <h3 className="text-xl font-semibold text-gray-600">{t('comingSoon')}</h3>
          <p className="text-gray-500">{t('underDevelopment')}</p>
        </div>
      );
    }
  };

  return (
    <div className={`flex bg-gray-50 min-h-screen ${currentLanguage === 'ar' ? 'rtl' : 'ltr'}`}>
      <Sidebar />
      <div className="flex-1 p-8">
        {renderContent()}
      </div>
      {chatOpen && <ChatAssistant />}
    </div>
  );
};

export default MiaKapitalPlatform;
