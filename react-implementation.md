1. Create the file structure:
```
/client
  /src
    /components
      CountryCard.js
      SearchBar.js
      RegionFilter.js
      StateList.js
      CityList.js
      Loading.js
    /pages
      CountriesList.js
      CountryDetails.js
    /services
      api.js
    /context
      AppContext.js
    App.js
    index.js
```

Let's go through each component:

1. `services/api.js`:
```javascript
const BASE_URL = 'http://localhost:5000/api';

export const api = {
  getCountries: async () => {
    const response = await fetch(`${BASE_URL}/countries`);
    return response.json();
  },

  getCountryByCode: async (code) => {
    const response = await fetch(`${BASE_URL}/countries/${code}`);
    return response.json();
  },

  searchCountries: async (term) => {
    const response = await fetch(`${BASE_URL}/countries/search?q=${term}`);
    return response.json();
  },

  getStatesByCountry: async (countryCode) => {
    const response = await fetch(`${BASE_URL}/states/${countryCode}`);
    return response.json();
  }
};
```

2. `components/CountryCard.js`:
```javascript
import React from 'react';
import { useNavigate } from 'react-router-dom';

export const CountryCard = ({ country }) => {
  const navigate = useNavigate();

  return (
    <div className="bg-white rounded-lg shadow p-6 hover:shadow-md transition-shadow">
      <h3 className="text-lg font-semibold mb-2">{country.name}</h3>
      <div className="text-sm text-gray-500">
        <p>Region: {country.region}</p>
        <p>States: {country.states?.length || 0}</p>
      </div>
      <button
        onClick={() => navigate(`/country/${country.code}`)}
        className="mt-4 text-blue-600 font-medium"
      >
        View Details →
      </button>
    </div>
  );
};
```

3. `components/SearchBar.js`:
```javascript
import React from 'react';
import { Search } from 'lucide-react';

export const SearchBar = ({ value, onChange }) => {
  return (
    <div className="relative flex-1">
      <Search className="absolute left-3 top-1/2 transform -translate-y-1/2 text-gray-400 w-5 h-5" />
      <input
        type="text"
        value={value}
        onChange={(e) => onChange(e.target.value)}
        placeholder="Search countries..."
        className="w-full pl-10 pr-4 py-2 border rounded-lg"
      />
    </div>
  );
};
```

4. `pages/CountriesList.js`:
```javascript
import React, { useState, useEffect } from 'react';
import { CountryCard } from '../components/CountryCard';
import { SearchBar } from '../components/SearchBar';
import { RegionFilter } from '../components/RegionFilter';
import { Loading } from '../components/Loading';
import { api } from '../services/api';

export const CountriesList = () => {
  const [countries, setCountries] = useState([]);
  const [loading, setLoading] = useState(true);
  const [search, setSearch] = useState('');
  const [region, setRegion] = useState('');

  useEffect(() => {
    loadCountries();
  }, []);

  const loadCountries = async () => {
    try {
      const data = await api.getCountries();
      setCountries(data);
    } catch (error) {
      console.error('Error loading countries:', error);
    } finally {
      setLoading(false);
    }
  };

  const filteredCountries = countries.filter(country => {
    const matchesSearch = country.name.toLowerCase().includes(search.toLowerCase());
    const matchesRegion = !region || country.region === region;
    return matchesSearch && matchesRegion;
  });

  if (loading) return <Loading />;

  return (
    <div className="container mx-auto px-4 py-8">
      <div className="mb-8">
        <div className="flex gap-4 flex-col sm:flex-row">
          <SearchBar value={search} onChange={setSearch} />
          <RegionFilter value={region} onChange={setRegion} />
        </div>
      </div>

      <div className="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-3 gap-6">
        {filteredCountries.map(country => (
          <CountryCard key={country.code} country={country} />
        ))}
      </div>
    </div>
  );
};
```

5. `pages/CountryDetails.js`:
```javascript
import React, { useState, useEffect } from 'react';
import { useParams, useNavigate } from 'react-router-dom';
import { ChevronLeft } from 'lucide-react';
import { StateList } from '../components/StateList';
import { CityList } from '../components/CityList';
import { Loading } from '../components/Loading';
import { api } from '../services/api';

export const CountryDetails = () => {
  const { code } = useParams();
  const navigate = useNavigate();
  const [country, setCountry] = useState(null);
  const [loading, setLoading] = useState(true);

  useEffect(() => {
    loadCountry();
  }, [code]);

  const loadCountry = async () => {
    try {
      const data = await api.getCountryByCode(code);
      setCountry(data);
    } catch (error) {
      console.error('Error loading country:', error);
    } finally {
      setLoading(false);
    }
  };

  if (loading) return <Loading />;

  return (
    <div className="container mx-auto px-4 py-8">
      <button
        onClick={() => navigate('/')}
        className="flex items-center text-blue-600 mb-6"
      >
        <ChevronLeft className="w-5 h-5" />
        Back to Countries
      </button>

      <h2 className="text-2xl font-bold mb-6">{country?.name}</h2>

      <div className="grid grid-cols-1 md:grid-cols-2 gap-8">
        <StateList states={country?.states || []} />
        <CityList cities={country?.states?.[0]?.cities || []} />
      </div>
    </div>
  );
};
```

6. `App.js`:
```javascript
import React from 'react';
import { BrowserRouter, Routes, Route } from 'react-router-dom';
import { CountriesList } from './pages/CountriesList';
import { CountryDetails } from './pages/CountryDetails';

function App() {
  return (
    <BrowserRouter>
      <div className="min-h-screen bg-gray-50">
        <header className="bg-white shadow">
          <div className="max-w-7xl mx-auto px-4 py-6">
            <h1 className="text-2xl font-bold text-gray-900">Country Explorer</h1>
          </div>
        </header>

        <Routes>
          <Route path="/" element={<CountriesList />} />
          <Route path="/country/:code" element={<CountryDetails />} />
        </Routes>
      </div>
    </BrowserRouter>
  );
}

export default App;
```

To complete the setup:

1. Install required dependencies:
```bash
npm install react-router-dom lucide-react
```

2. Add Tailwind CSS to your project:
```bash
npm install -D tailwindcss postcss autoprefixer
npx tailwindcss init -p
```

