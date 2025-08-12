# Example: Adding a New Feature

This example demonstrates how to use the Cursor rules when adding a new feature to your AI App Bootstrap project.

## 🎯 Scenario

You want to add a new AI-powered "Smart Search" feature to your application that allows users to search through documents using natural language queries.

## 📋 Using the Rules

### **1. Reference Project Context**
```
@cursor-rules:project-context
I want to add a Smart Search feature. This should follow our AI-First design principles and integrate with our existing architecture.
```

### **2. Follow Coding Standards**
```
@cursor-rules:coding-standards
Please implement this feature following our Python coding standards with proper type hints, docstrings, and error handling.
```

### **3. Use File Templates**
```
@cursor-rules:file-templates
Create the necessary files following our standard project structure for new features.
```

## 🏗️ Implementation Request

Here's how you would prompt Cursor to implement this feature:

```
I want to add a Smart Search feature to my AI application. Here's what I need:

1. A new API endpoint `/api/search` that accepts natural language queries
2. Integration with OpenAI's embedding API for semantic search
3. A service layer that handles the search logic
4. Database models for storing search results and user queries
5. Proper error handling and validation

Please implement this following our AI App Bootstrap patterns:
- Use our project structure and naming conventions
- Follow our coding standards for Python
- Include proper testing
- Add appropriate documentation
- Use our validation framework for input validation

The feature should integrate with our existing user authentication system and follow our security patterns.
```

## 📁 Expected File Structure

Cursor should create these files following our patterns:

```
app/
├── api/
│   └── search.py              # Search API endpoints
├── services/
│   └── search_service.py      # Search business logic
├── models/
│   └── search.py              # Search data models
├── schemas/
│   └── search.py              # Pydantic schemas
└── tests/
    └── test_search.py         # Search tests
```

## 🔧 Code Generation Examples

### **API Endpoint (Expected Output)**
```python
"""
Search API endpoints for smart document search functionality.
"""

from fastapi import APIRouter, Depends, HTTPException
from typing import List, Optional

from app.core.auth import get_current_user
from app.schemas.search import SearchQuery, SearchResult
from app.services.search_service import SearchService
from app.models.user import User

router = APIRouter(prefix="/search", tags=["search"])

@router.post("/", response_model=List[SearchResult])
async def search_documents(
    query: SearchQuery,
    current_user: User = Depends(get_current_user),
    search_service: SearchService = Depends()
) -> List[SearchResult]:
    """
    Search documents using natural language queries.
    
    Args:
        query: Search query with text and filters
        current_user: Authenticated user making the request
        search_service: Search service instance
    
    Returns:
        List of search results matching the query
    
    Raises:
        HTTPException: If search fails or query is invalid
    """
    try:
        results = await search_service.search_documents(
            query=query.text,
            user_id=current_user.id,
            filters=query.filters
        )
        return results
    except ValueError as e:
        raise HTTPException(status_code=400, detail=str(e))
    except Exception as e:
        raise HTTPException(status_code=500, detail="Search failed")
```

### **Service Layer (Expected Output)**
```python
"""
Search service for handling document search operations.
"""

import logging
from typing import List, Optional
from openai import OpenAI

from app.models.search import SearchResult, SearchQuery
from app.core.config import settings
from app.utils.text_processing import preprocess_query

logger = logging.getLogger(__name__)

class SearchService:
    """Service for handling smart document search."""
    
    def __init__(self):
        self.openai_client = OpenAI(api_key=settings.openai_api_key)
    
    async def search_documents(
        self,
        query: str,
        user_id: int,
        filters: Optional[dict] = None
    ) -> List[SearchResult]:
        """
        Search documents using semantic search.
        
        Args:
            query: Natural language search query
            user_id: ID of the user performing the search
            filters: Optional search filters
            
        Returns:
            List of search results
            
        Raises:
            ValueError: If query is invalid
        """
        # Input validation
        if not query or len(query.strip()) < 2:
            raise ValueError("Search query must be at least 2 characters")
        
        # Preprocess query
        processed_query = preprocess_query(query)
        
        try:
            # Generate embeddings for semantic search
            embedding = await self._generate_embedding(processed_query)
            
            # Perform search using embeddings
            results = await self._search_by_embedding(embedding, filters)
            
            # Log search activity
            await self._log_search_activity(user_id, query, len(results))
            
            return results
            
        except Exception as e:
            logger.error(f"Search failed for query '{query}': {e}")
            raise
```

## ✅ Quality Checklist

When Cursor generates code, ensure it includes:

- [ ] **Proper imports** and dependencies
- [ ] **Type hints** for all functions and parameters
- [ ] **Comprehensive docstrings** explaining purpose and usage
- [ ] **Error handling** with appropriate HTTP status codes
- [ ] **Input validation** using Pydantic schemas
- [ ] **Logging** for debugging and monitoring
- [ ] **Testing** with proper test structure
- [ ] **Security** following our authentication patterns
- [ ] **Documentation** in code and README updates

## 🔄 Iteration Process

1. **Generate initial code** using the rules
2. **Review and refine** the generated code
3. **Add missing pieces** if needed
4. **Test the implementation** thoroughly
5. **Update documentation** to reflect changes
6. **Commit and deploy** following our workflow

## 💡 Tips for Best Results

- **Be specific** about requirements and constraints
- **Reference multiple rules** for complex features
- **Provide context** about existing code and patterns
- **Ask for explanations** of generated code
- **Request improvements** if the first attempt isn't perfect
- **Use the examples** as templates for similar features

---

*This example shows how to effectively use Cursor rules to implement new features while maintaining consistency with your AI App Bootstrap project patterns.*
