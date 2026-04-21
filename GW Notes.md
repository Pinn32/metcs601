---
date: 2026-04-20
---

# 2026-04-20

1. NewsGrid + NewsCard

```tsx
// NewsCard: Displays a single news article as a card in the news grid.  
  
import { useState } from "react";  
import Link from "next/link";  
import styled from "styled-components";  
import type { Article } from "@/types";  
  
  
// Styled Components  
  
// Card container with subtle shadow and hover elevation effect  
const Card = styled.div`  
  height: 100%;  background: var(--color-bg);  border-radius: 8px;  overflow: hidden;  box-shadow: 0 1px 3px rgba(0, 0, 0, 0.08);  transition: box-shadow 0.2s;    &:hover {  
    box-shadow: 0 4px 12px rgba(0, 0, 0, 0.2);  }`;  
  
// Image container with fixed height and grey fallback background  
const ImageWrapper = styled.div`  
  width: 100%;  height: calc(180px + 1vh);  background: var(--color-bg-subtle);  display: flex;  align-items: center;  justify-content: center;  color: var(--color-text-placeholder);  overflow: hidden;`;  
  
// Thumbnail image that fills its container with cover mode  
const ThumbnailImage = styled.img`  
  width: 100%;  height: 100%;  object-fit: cover;`;  
  
// Content area below the image  
const Content = styled.div`  
  padding: 1rem 1.5rem 1.5rem;`;  
  
// Category label displayed above the title  
const Category = styled.span`  
  font-size: 12px;  color: var(--color-primary);  font-weight: 500;`;  
  
// Article title, clamped to 2 lines with ellipsis overflow  
const Title = styled.h3`  
  font-size: 16px;  font-weight: 600;  color: var(--color-text);  margin: 8px 0;  line-height: 1.4;  display: -webkit-box;  -webkit-line-clamp: 2;  -webkit-box-orient: vertical;  overflow: hidden;`;  
  
// Article summary, clamped to 2 lines  
const Summary = styled.p`  
  font-size: 14px;  color: var(--color-text-subtle);  line-height: 1.5;  display: -webkit-box;  -webkit-line-clamp: 2;  -webkit-box-orient: vertical;  overflow: hidden;  margin-bottom: 12px;`;  
  
// Footer row displaying author and date  
const Meta = styled.div`  
  display: flex;  justify-content: space-between;  align-items: center;  font-size: 12px;  color: var(--color-text-placeholder);`;  
  
// Wrapper link that makes the entire card clickable  
const StyledLink = styled(Link)`  
  text-decoration: none;  display: block;`;  
  
/* --- Component --- */  
  
export default function NewsCard({ article }: { article: Article }) {  
  // Track image load errors to show fallback text  
  const [imgError, setImgError] = useState(false);  
  
  return (  
    <StyledLink href={`/article/${article.id}`}>  
      <Card>  
        <ImageWrapper>  
          {/* Show article image if available and not errored; otherwise show fallback */}  
          <ThumbnailImage  
            src={article.imageUrl && !imgError ? article.imageUrl : "/placeholder.png"}  
            alt={article.title}  
            onError={() => setImgError(true)}  
          />  
        </ImageWrapper>  
        <Content>  
          <Category>{article.category}</Category>  
          <Title>{article.title}</Title>  
          <Summary>{article.summary}</Summary>  
          <Meta>  
            <span>{article.author}</span>  
            <span>  
              {new Date(article.publishedAt).toLocaleDateString("en-US")}  
            </span>  
          </Meta>  
        </Content>  
      </Card>  
    </StyledLink>  
  );  
}
```

```tsx
// NewsGrid
export const NewsGrid = styled.div`  
    display: grid;    grid-template-columns: repeat(auto-fill, minmax(320px, 1fr));    gap: 2rem;`;
```

```tsx
// Home page
// Home Page of the App  
  
"use client";  
  
import { useState } from "react";  
import { PageContainer, PageTitle, NewsGrid } from "@/components/primitives";  
import NewsCard from "@/components/news/NewsCard";  
import CategoryTabs from "@/components/news/CategoryTabs";  
import SearchBar from "@/components/layout/SearchBar";  
import AsyncBoundary from "@/components/ui/AsyncBoundary";  
import styled from "styled-components";  
import type { Article } from "@/types";  
import { useFetch } from "@/hooks/useFetch";  
  
// Styled header  
const Header = styled.div`  
  margin-bottom: 2%;`;  
  
export default function Home() {  
  // State to track the selected news category (default is "all")  
  const [category, setCategory] = useState("all");  
  // Fetch news data based on the selected category  
  // The API endpoint updates whenever `category` changes  const { data, loading, error } = useFetch<{ articles: Article[] }>(  
    `/api/news?category=${category}`  
  );  
  // Fallback to empty array if data is not yet available  
  const news = data?.articles ?? [];  
  
  // Render the UI  
  return (  
    <PageContainer>  
      {/* Page header section with title and search */}  
      <Header>  
        <PageTitle>Today&apos;s News</PageTitle>  
        <SearchBar />  
      </Header>  
      {/* Category filter tabs */}  
      <CategoryTabs activeCategory={category} onCategoryChange={setCategory} />  
      {/* Handle loading and error states while fetching data */}  
      <AsyncBoundary loading={loading} error={error}>  
        {/* Display list of news articles */}  
        <NewsGrid>  
          {news.map((article) => (  
            // Render individual news article card  
            <NewsCard key={article.id} article={article} />  
          ))}  
        </NewsGrid>  
      </AsyncBoundary>  
    </PageContainer>  
  );  
}
```

2. BookmarkList

```tsx
// BookmarkList

import { useState } from "react";  
import Link from "next/link";  
import { useBookmarks } from "@/context/BookmarkContext";  
import styled from "styled-components";  
import type { BookmarkData } from "@/types";  
  
/* --- Styled Components --- */  
  
// Responsive grid container for bookmark cards  
const Container = styled.div`  
  display: grid;  grid-template-columns: repeat(auto-fill, minmax(320px, 1fr));  gap: 2rem;`;  
  
// Card container with subtle shadow and hover elevation effect  
const Card = styled.div`  
  height: 100%;  background: var(--color-bg);  border-radius: 8px;  overflow: hidden;  box-shadow: 0 1px 3px rgba(0, 0, 0, 0.08);  transition: box-shadow 0.2s;  position: relative;    &:hover {  
    box-shadow: 0 4px 12px rgba(0, 0, 0, 0.2);  }`;  
  
// Thumbnail image container  
const ImageWrapper = styled.div`  
  width: 100%;  height: 160px;  background: var(--color-bg-subtle);  display: flex;  align-items: center;  justify-content: center;  color: var(--color-text-placeholder);  overflow: hidden;`;  
  
// Bookmark thumbnail image  
const ThumbnailImage = styled.img`  
  width: 100%;  height: 100%;  object-fit: cover;`;  
  
// Card content area below the image  
const Content = styled.div`  
  padding: 16px;`;  
  
// Category label  
const Category = styled.span`  
  font-size: 0.75rem;  color: var(--color-primary);`;  
  
// Bookmark title  
const Title = styled.h3`  
  font-size: 1rem;  font-weight: 600;  color: var(--color-text);  margin: 8px 0;`;  
  
// Author and date metadata  
const Meta = styled.div`  
  font-size: 12px;  color: var(--color-text-placeholder);`;  
  
// Circular delete button positioned at top-right corner of the card  
const DeleteButton = styled.button`  
  position: absolute;  top: 8px;  right: 8px;  width: 32px;  height: 32px;  border-radius: 50%;  border: none;  background: rgba(255, 255, 255, 0.9);  color: var(--color-danger);  cursor: pointer;  display: flex;  align-items: center;  justify-content: center;  
  &:hover {    background: var(--color-danger);    color: var(--color-text-inverse);  }`;  
  
// Empty state shown when the user has no bookmarks  
const EmptyState = styled.div`  
  text-align: center;  padding: 64px;  color: var(--color-text-placeholder);  grid-column: 1 / -1;`;  
  
// Makes the entire card clickable as a link  
const StyledLink = styled(Link)`  
  text-decoration: none;  display: block;`;  
  
/* --- Sub-Component --- */  
  
/**  
 * BookmarkImage - Handles individual image error state. * Extracted as a separate component so each bookmark card can independently * track its own image load error via useState (required by React's Rules of Hooks). */function BookmarkImage({ src, alt }: { src: string; alt: string }) {  
  const [error, setError] = useState(false);  
  const imgSrc = src && !error ? src : "/placeholder.png";  
  return (  
    <ThumbnailImage src={imgSrc} alt={alt} onError={() => setError(true)} />  
  );  
}  
  
/* --- Component --- */  
  
export default function BookmarkList({  
  bookmarks,  
}: {  
  bookmarks: BookmarkData[];  
}) {  
  const { removeBookmark } = useBookmarks();  
  
  // Show empty state if user has no bookmarks  
  if (bookmarks.length === 0) {  
    return <EmptyState>No bookmarked articles yet</EmptyState>;  
  }  
  
  return (  
    <Container>  
      {bookmarks.map((bookmark) => (  
        <Card key={bookmark._id}>  
          {/* Delete button to remove this bookmark */}  
          <DeleteButton  
            onClick={() => removeBookmark(bookmark._id)}  
            title="Remove bookmark"  
          >  
            &times;  
          </DeleteButton>  
          {/* Card content links to the article detail page */}  
          <StyledLink href={`/article/${bookmark.articleId}`}>  
            <ImageWrapper>  
              <BookmarkImage src={bookmark.imageUrl} alt={bookmark.title} />  
            </ImageWrapper>  
            <Content>  
              <Category>{bookmark.category}</Category>  
              <Title>{bookmark.title}</Title>  
              <Meta>  
                {bookmark.author} &middot;{" "}  
                {new Date(bookmark.publishedAt).toLocaleDateString("en-US")}  
              </Meta>  
            </Content>  
          </StyledLink>  
        </Card>  
      ))}  
    </Container>  
  );  
}
```

```tsx
// Bookmarks Page: display user-tailored bookmarked news  
  
"use client";  
  
import { useBookmarks } from "@/context/BookmarkContext";  
import { PageContainer, PageTitle } from "@/components/primitives";  
import BookmarkList from "@/components/layout/BookmarkList";  
import styled from "styled-components";  
  
// Styled component for displaying the number of bookmarks next to the title  
const Count = styled.span`  
    color: var(--color-text-muted);    font-size: 1rem;    font-weight: 400;`;  
  
export default function BookmarksPage() {  
    // Get bookmarked articles from global context  
    const { bookmarks } = useBookmarks();  
  
    return (  
    <PageContainer>  
        <PageTitle>  
            My Bookmarks <Count>({bookmarks.length})</Count> {/* Display total number of bookmarks */}  
        </PageTitle>  
        <BookmarkList bookmarks={bookmarks} />  
    </PageContainer>  
    );  
}
```

