# [Web] Build Pools list page

## Description

This PR implements the `/pools` page for the Linkora web application, providing a comprehensive listing of all community pools with their key details and governance information.

The implementation includes:
- **Pools List Page** (`packages/web/app/pools/page.tsx`) - Main page displaying all community pools in a responsive grid
- **Pool Card Component** (`packages/web/app/components/pools/PoolCard.tsx`) - Reusable card component showing pool details
- **Supporting Components** - Empty state, threshold badge, and data fetching hooks

## Changes Made

### New/Modified Files

1. **`packages/web/app/pools/page.tsx`**
   - Main pools listing page with server-side data fetching
   - Responsive grid layout (auto-fill, minmax 300px)
   - Refresh button to reload pools
   - Create Pool CTA button
   - Error handling with retry mechanism
   - Loading state with skeleton cards
   - Empty state when no pools exist
   - Pool count display with aria-live for accessibility

2. **`packages/web/app/components/pools/PoolCard.tsx`**
   - Displays pool ID with icon
   - Shows token symbol and address (truncated)
   - Formatted token balance with decimal handling
   - Governance threshold badge (M-of-N multisig)
   - Status badge (Active/Empty)
   - Navigation link to pool detail page
   - Loading skeleton variant for better UX
   - Hover effects: border highlight, shadow, and lift animation

### Supporting Components

3. **`packages/web/app/components/pools/PoolEmptyState.tsx`**
   - Three variants: no-pools, zero-balance, not-found
   - SVG illustrations for each state
   - CTA buttons for next actions

4. **`packages/web/app/components/pools/ThresholdBadge.tsx`**
   - Compact and full variants
   - M-of-N multisig visualization
   - Progress bar with admin signature indicators

5. **`packages/web/app/hooks/usePools.ts`**
   - `useAllPools()` - Fetches all pools
   - `usePool(poolId)` - Fetches single pool details
   - `useTokenMeta(address)` - Fetches token metadata
   - Utility functions: truncateAddress, formatTokenAmount, parseTokenAmount

## Acceptance Criteria

- ✅ **Pools listed with ID, token symbol, and balance**
  - PoolCard component displays all three data points
  - Token amounts formatted with proper decimal handling
  - Token symbols fetched from mock contract data

- ✅ **Tapping a card navigates to the pool detail page**
  - Link component routes to `/pools/${pool.pool_id}`
  - Pool detail page already exists at `packages/web/app/pools/[id]/page.tsx`
  - Proper aria-label for accessibility

- ✅ **Empty state handled**
  - PoolEmptyState component shown when no pools exist
  - Provides CTA button to create a new pool
  - Link to `/pools/new` for pool creation

## Design & UX Features

### Responsive Design
- Grid layout: `repeat(auto-fill, minmax(300px, 1fr))`
- Adapts smoothly from mobile to desktop
- Maintains 44px minimum touch targets per WCAG guidelines

### Visual Polish
- Hover effects: border color change, shadow, and 2px lift
- Loading skeletons with shimmer animation
- Status badges (Active/Empty) with color coding
- Clean typography hierarchy
- Dark mode support via CSS variables

### Accessibility
- Semantic HTML: `<article>`, `<main>`, `<header>`
- ARIA labels on all interactive elements
- `aria-live="polite"` for pool count updates
- `aria-busy` during loading states
- Status badges include aria-labels
- 44px minimum touch targets for all interactive elements

### Performance
- Skeleton loading cards improve perceived performance
- Mock data with TODO comments for SDK integration
- Efficient re-rendering with React hooks
- No unnecessary re-fetches

## Testing

### Manual Testing
1. Navigate to `/pools` - Should display all available pools
2. Verify pool cards show:
   - Pool ID with icon
   - Token symbol and address
   - Formatted balance
   - Governance threshold
   - Status badge
3. Click refresh button - Should reload pool list
4. Click create pool button - Should navigate to `/pools/new`
5. Click a pool card - Should navigate to `/pools/{poolId}`
6. Verify empty state displays when no pools exist
7. Test on mobile viewport - Should stack in single column

### UI States
- ✅ Loading state (skeleton cards)
- ✅ Success state (pool grid)
- ✅ Error state (error banner with retry)
- ✅ Empty state (illustrated empty state)

### Accessibility Testing
- ✅ Keyboard navigation works
- ✅ Screen reader announces all labels
- ✅ Focus indicators visible
- ✅ Sufficient color contrast

## Design Tokens Used

All styling uses CSS variables from `globals.css`:
- Colors: `--color-primary`, `--color-bg`, `--color-border`, etc.
- Spacing: `--space-1` through `--space-16`
- Typography: `--text-xs` through `--text-4xl`
- Border radius: `--radius-sm` through `--radius-full`

## Browser Compatibility

- Modern browsers with CSS Grid support
- Chrome, Firefox, Safari, Edge (latest versions)
- Mobile browsers (iOS Safari, Chrome Mobile)

## Breaking Changes

None. This is a new feature that doesn't modify existing functionality.

## Related Issues

Closes #300
